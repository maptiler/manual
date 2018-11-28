============
Installation
============

MapTiler Engine is part of `MapTiler Desktop PRO`_, available as command line utility `maptiler`

.. _MapTiler Desktop PRO: https://www.maptiler.com/demo/


Windows
=======
Download the setup.exe (sent by an email) and start the installation wizard. After it is finished a new section is added to Start -> All Programs -> MapTiler Desktop Pro, and it is possible to use the command `maptiler` from the standard Command Prompt application in Windows.


macOS
=======

Download the installation disk image (DMG, sent by an email) and after opening the disk image in macOS you should drag the "MapTiler Desktop Pro" icon into your "Application" folder. The command line interface can be started from Terminal application as: `/Applications/MapTiler\ Desktop\ Pro.app/Contents/MacOS/maptiler`.

All versions of macOS higher than 10.9 (Mavericks) are supported.

Linux
======
Download the installation package (.deb or .rpm) for your Linux distribution from the link sent by an email. The packages are compiled against the standard out-of-the-box GDAL installation available for your Linux distribution (typically it is the package gdal automatically available via apt or yum/dnf).

In case a different version of the GDAL library is required for your project, we can provide you with binaries compiled for such a version on request_.

.. _request: https://www.maptiler.com/demo/

Supported Linux distributions are: Debian, Ubuntu, Redhat and Fedora (the rpms are dependent on GDAL from EPEL repository).
Another option for running the latest MapTiler Engine with the latest GDAL on any Linux distribution is via the `Docker container`_.

.. _Docker container: https://hub.docker.com/r/maptiler/engine/
