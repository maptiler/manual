=======
Demo version
=======

Setting CPU limits
=======

MapTiler Engine is a multi-threaded program. By default, it will use all the CPUs you have and print the information about the cores. You can also set a limit on the defined number of cores yourself with the `-P` option.

`-P [num]`
 Set number of CPU cores to be used for render process.

Example: ::

  maptiler -P 4 ...

This is especially practical to evaluate the demo application before purchasing a license for a particular number of cores.

The modern CPUs has multiple cores and support of Hyperthreading - which provides multiple logical CPUs per core. This way MapTiler Engine can provide higher performance with `-P` 4 even on a dual-core computer.


Demo trial extension
=======

The demo version of MapTiler Engine is fully usable for 30 days after the first start. In case this trial period for testing the software before purchase expires and you would like to continue to test the demo, it is necessary to contact Klokan Technologies GmbH and request a trial extension key.

Such key can be then used with the parameter:

`-extend_trial [TRIAL-KEY]`
 Extends MapTiler Engine demo trial period.

And the demo can be then used during an extended time period.


Software activation online
========

After a purchase of the software, when you receive your license key, it is necessary to activate MapTiler Engine. After activation, the demo version no longer enforces the “MapTiler DEMO” overlays.

To do the online activation, use the following command:

`-activate [YOUR-OWN-LICENSE-KEY]`
 Activates MapTiler Engine with proper license key. You can get your `License key online here <https://www.maptiler.com/pricing/>`_.

Example: ::

  maptiler -activate YOUR-OWN-LICENSE-KEY

**In case you require to reinstall the computer, use** -deactivate **command to be able to re-activate the license later on.**

The software activation for MapTiler Desktop Pro via GUI `is described here <https://support.maptiler.com/i56-license-activation>`_.


Software activation in a virtual machine
--------

The activation process for MapTiler Engine running in a virtual machine requires online activation only via environment variable `MAPTILER_LICENSE`.
The software will be automatically deactivated in the end.

Example on Windows OS ::

 set MAPTILER_LICENSE=YOUR-OWN-LICENSE-KEY
 maptiler ...


Example on Linux / macOS ::

 export MAPTILER_LICENSE=YOUR-OWN-LICENSE-KEY
 maptiler ...

**Notice:** This activation process via environment variable requires **not-activated MapTiler Desktop Pro** (or **not-activated MapTiler Engine**) instance on that computer! Starting MapTiler Desktop application without setting this environment variable `MAPTILER_LICENSE`, you should see either **MapTiler Desktop Pro Demo**, or **Your trial period has expired** error message.


Software deactivation online
========

You can deactivate MapTiler Engine via command line, in order to transfer the application to another computer.
Note that after the deactivation, MapTiler Engine will continue to run in DEMO mode, but the trial period of 30 days will **NOT** start again.

To do the online deactivation, use the following command:

`-deactivate`
 Deactivates activated MapTiler Engine software. MapTiler Engine can be used in Demo mode, if trial period has not expired.

Example: ::

  maptiler -deactivate

You can deactivate MapTiler Desktop Pro `in GUI via License dialog described here <https://support.maptiler.com/i449-license-deactivation>`_.


License information
========

To check your license information, use the following command:

`-license`
 Print your license information for activated MapTiler Engine.

Example ::

  > maptiler -license
  Email: <your email>
  License: <your license key>
  Purchased CPU cores: 4
  Maximum usable cores: 4


  maptiler -license
  Remaining 0 days on the trial.
  Maximum usable cores: 4


This argument may print additional texts, depending on your License, for example: ::

  Time limited license!
  X days remaining.

  X days remaining in your maintenance period.

  Your maintenance period has expired. You can't upgrade to newer versions - please contact support@maptiler.com.
