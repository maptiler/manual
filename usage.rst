======
Usage
======

Getting started
---------------

The main purpose of the software is to generate tiles in a defined tiling system, given some options and inputs. The only mandatory option is -o for the output directory. This directory must not exist yet when you run MapTiler, to avoid overwriting existing data by mistake. As input, you can just provide the source dataset filename(s) [1]_.::


 ￼maptiler -o output_directory input_file.ext

an example:




.. [1] Depending on your operating system you may need to call the command differently then just maptiler, typically on Linux and Mac in actual directory as ./maptiler and on Windows as maptiler.exe.
