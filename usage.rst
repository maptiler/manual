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
output_directory/z/x/y.ext






.. [#] Depending on your operating system you may need to call the command differently then just maptiler, typically on Linux and Mac in actual directory as ./maptiler and on Windows as maptiler.exe.
