======
Usage
======

Getting started
---------------

The main purpose of the software is to generate tiles in a defined tiling system, given some options and inputs. The only mandatory option is -o for the output directory. This directory must not exist yet when you run MapTiler, to avoid overwriting existing data by mistake. As input, you can just provide the source dataset filename(s) [#]_. ::

 ￼maptiler -o output_directory input_file.ext

an example: ::

 maptiler -o tiles map.tif

To render more files at once, just specify them one after another: ::

 maptiler -o output_directory input1.tif input2.tif input3.tif
 
If you start the maptiler without arguments or with -help option, it will print all available commands: ::

 maptiler -help
 
Output
------

The default behaviour of MapTiler is to write tiles each into its own file, under the directory structure:

 `output_directory/z/x/y.ext`

Where z is the zoom level, and x and y tile coordinates on relevant zoom level in the tiling profile.

The produced directory structure contains also a simple HTML viewer and description of the dataset in metadata.json, compatible with mb-util and TileServer_ project supporting OGC WMTS standard.

.. _TileServer: https://github.com/klokantech/tileserver-php/

MapTiler supports direct output of the rendered map tiles into an SQLite database (MBTiles format). This simplifies transfer and management of the tilesets and is practical for mobile applications.

MapTiler Command Structure
------
.. image:: img/maptiler_command_structure.jpg

The global options apply to all input files, in other words:

**Only arguments specified BEFORE the input filenames are applied to all files!**

Arguments which should be applied only to a single file are specified AFTER the name of such file (for example zoom level range specific only to that file) and has higher priority then the global options.

Available output options
------

Tiling profile / Tile Matrix Set

A global option defining the output system of tiles - the target coordinate system, tile pixel size, etc. MapTiler comes with three predefined most popular systems and possibility to specify a custom profile.

`-mercator`
 DEFAULT. The spherical mercator tile profile compatible with Google, Bing, Yahoo Maps, MapQuest, OpenStreetMap, and mobile maps on iOS and Android. This is the most commonly used profile. It uses coordinate system defined as EPSG:3857 or EPSG:900913. Details at: http:// www.maptiler.org/google-maps-coordinates-tile-bounds-projection/ 2








.. [#] Depending on your operating system you may need to call the command differently then just maptiler, typically on Linux and Mac in actual directory as ./maptiler and on Windows as maptiler.exe.

