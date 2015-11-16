======
Usage
======

Getting started
========
The main purpose of the software is to generate tiles in a defined tiling system, given some options and inputs. The only mandatory option is -o for the output directory. This directory must not exist yet when you run MapTiler, to avoid overwriting existing data by mistake. As input, you can just provide the source dataset filename(s) [#]_. ::

 ￼maptiler -o output_directory input_file.ext

an example: ::

 maptiler -o tiles map.tif

To render more files at once, just specify them one after another: ::

 maptiler -o output_directory input1.tif input2.tif input3.tif
 
If you start the maptiler without arguments or with -help option, it will print all available commands: ::

 maptiler -help
 
Output
======

The default behaviour of MapTiler is to write tiles each into its own file, under the directory structure:

 `output_directory/z/x/y.ext`

Where z is the zoom level, and x and y tile coordinates on relevant zoom level in the tiling profile.

The produced directory structure contains also a simple HTML viewer and description of the dataset in metadata.json, compatible with mb-util and TileServer_ project supporting OGC WMTS standard.

.. _TileServer: https://github.com/klokantech/tileserver-php/

MapTiler supports direct output of the rendered map tiles into an SQLite database (MBTiles format). This simplifies transfer and management of the tilesets and is practical for mobile applications.

MapTiler Command Structure
=======
.. image:: /images/maptiler_command_structure.jpg

The global options apply to all input files, in other words:

**Only arguments specified BEFORE the input filenames are applied to all files!**

Arguments which should be applied only to a single file are specified AFTER the name of such file (for example zoom level range specific only to that file) and has higher priority then the global options.

Available output options
======

Tiling profile / Tile Matrix Set
-------
A global option defining the output system of tiles - the target coordinate system, tile pixel size, etc. MapTiler comes with three predefined most popular systems and possibility to specify a custom profile.

`-mercator`
 DEFAULT. The spherical mercator tile profile compatible with Google, Bing, Yahoo Maps, MapQuest, OpenStreetMap, and mobile maps on iOS and Android. This is the most commonly used profile. It uses coordinate system defined as EPSG:3857 or EPSG:900913. Details at: http://www.maptiler.org/google-maps-coordinates-tile-bounds-projection/ [#]_.

`-mercator@2x`
 To create high-resolution Retina / HiDPI tiles with double pixel resolution.

In case you wish to use other tiling system, you must specify it as the first command on the command line. These are the alternatives:

`-geodetic`
 WGS84 Plate Caree / Unprojected. Compatible with most existing WMS servers, OpenLayers base map.
 
`-gearth`
 Tile profile specific for Google Earth according the KML SuperOverlay definition. 

`-custom`
 You can specify your own tiling system. See the appropriate section under the Advanced options chapter for more information.

`-raster`
 Rendering raster files without a need of georeference is possible as well.

`-garmin`
 To produce format for Garmin GPS devices, with a size 1024x1024 pixels and output to .kml file. Afterwards, pack the tiles and the .kml tiles into a .zip archive, change its extension to .kmz and save it into the device.

Example: command for producing tiles for use with Google Earth: ::

 ￼maptiler -o tiles -gearth map.tif

**Zoom levels**

`-zoom`
 This option determines which layers of the tile pyramid will be generated. Default is the "native" level as calculated from image resolution. In case you need to add additional zoom levels, you can either define them as absolute numeric values or as relative numbers to the “native” levels with prefix + and -.
 
 Each input file can have it’s own explicit option for zoomlevels. 
 
Example: zoom levels are automatically calculated as eg. 1 - 5 ::

 maptiler -o tiles map.tif

Example: zoom levels are explicitly set to be 3 - 5 ::

 maptiler -o tiles map.tif -zoom 3 5

Example: zoom levels are set to be 1 - 6 with relative value to native zoomlevels ::

 maptiler -o tiles map.tif -zoom +0 +1

**Tile formats**

The produced tiles can be saved in one of several image format. MapTiler includes optimization of the final filesize and used number of colors (quantization), to minimize the disk size occupied by the rendered maps as well as the time necessary to transfer the maps to clients once the tiles are online.

Formats with support for transparency are:

`-f png8a`
 DEFAULT. Paletted RGBA PNG image. 
 
`-f png or -f png32`
 RGBA PNG image 
 
Non-transparent formats are: 

`-f jpg or -f jpeg`
 Progressive JPEG image in the YCbCr color space

`-f png8`
 Paletted RGB PNG image

`-f png24`
 RGB PNG image

**Tile transparency or a background color**

No matter what input datasets you specify, after transforming them into the tiling profile projection, MapTiler will handle them as RGBA images. The transparency can come from the image itself as an alpha channel (with support for partly transparent areas), it can be derived from a selected color (so called NODATA color), or can be just a result of the transformation with the GDAL warping algorithm - for areas without available input data.

If the tile is completely transparent it is never saved to the disk to save the storage space.

If all of the pixels are fully visible (eg. opaque, maximum alpha is 255), the alpha channel is discarded and the tile is marked as non-transparent / opaque. Otherwise the tile is marked as partly transparent with alpha.

If partly transparent tiles are saved into a tile format without support for transparency (such as JPEG specified with -f jpg option) then the background color is applied. Default background color is white (255,255,255), but you can specify your own with the option:￼

`-bg [r] [g] [b]`
 The color of the background replacing transparency in the non-transparent tile formats.

For example: ::

 ￼maptiler -f png8 -bg 0 128 0 ...


**Tile store format**

`-store dir|mbtiles`
 This option enforces the form of storage which is used for saving the rendered tiles. Possible options are the directory (dir) and the MBTiles (mbtiles). The default is the directory, but in case the -o parameter ends with .mbtiles then rendering into mbtiles is selected. This option specify the store form explicitely.

 Note: for more details on this subject read the section Output in the chapter Usage above.
 
**Hybrid tile format**

MapTiler allows rendering into a hybrid tile format, so that transparent tiles are using transparent format (such as PNG) and tiles without any transparency at all are saved into a different format (such as JPEG). For aerial photos overlays or other datasets this can mean significant saving of the storage, but the client application and hosting service must be able to handle this case.

Example of usage: ::

 ￼maptiler -f hybrid jpg png8a ...

**Tile quality**

There are some options to specify parameters of the conversion into image formats, which can significantly reduce size of produced tiles by degrading the output.

`-jpg_quality`
 The quality of JPEG compression. Number between 10 and 95. Default is 85.

`-quant_quality`
 The quality of quantization. Number between 1 and 100. Default is 85.

`-quant_speed`
 Higher speed levels disable expensive algorithms and reduce quantization precision. Speed 1 gives marginally better quality at significant CPU cost. Speed 10 has usually 5% lower quality, but is 8 times faster than the default. Default is 8.
 
 *If you experience issues with the visual quality of generated tiles with quantization involved try to set -quant_speed to lower values.*
 
`-quant_colors`
 The number of colors in the PNG palette. Number between 1 and 256. Default is 256.

Example of the rendering of a seamless map out of file map1.tif and map2.tif into tiles with internal palette with optimal colors with higher visual : ::

 ￼maptiler -o tiles -f png8a -quant_quality 90 -quant_speed 4 map1.tif map2.tif
 
**Watermark**

`-watermark [image_file.png]`
 It is possible to place your own watermark over rendered tiles to protect the online maps. The file should be smaller then a size of tiles. It is placed on a random position and burned into tiles.

A nice watermark file can be easily generated online by calling the Google Chart API:   
http://chart.apis.google.com/chart?chst=d_text_outline&chld=FFFFFF|11|h|000000|b|%C2%A9%20ABC

By replacing ABC in the end of this url a custom text phrase can be specified. We recommend to set the transparency of such watermark file by using a Photoshop or similar tool before applying it with MapTiler.

Example of usage of the watermark: ::

 ￼maptiler -o tiles -watermark watermark_image.png map.tif







.. [#] Depending on your operating system you may need to call the command differently then just maptiler, typically on Linux and Mac in actual directory as ./maptiler and on Windows as maptiler.exe.

.. [#] MapTiler uses Google XYZ naming of tiles, while older open-source MapTiler and GDAL2Tiles used the TMS nam- ing (with flipped Y axis). In case you need the older TMS naming there is an option -tms for back compatibility.
