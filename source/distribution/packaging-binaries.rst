.. _packaging-binaries:

Packaging Binaries
==================

Overview
--------

The ``bdist_apps`` command will run
the ``build_apps`` command and then
package the resulting builds. Currently, the only packaging options available
are archives (e.g, zip, tar.gz, etc.). Adding support for installers is
planned for the future.

The ``bdist_apps`` command will make
use of ``build_apps.build_base`` and
``build_apps.platforms``. The following options
are unique to ``bdist_apps``:

installers: a dictionary of installers/packages to create for each platform (keys are strings for the platform, values are strings representing installers, defaults to gztar for manylinux1_x86_64/manylinux1_i386 and zip for everything else)

Installers
----------

The list of available installers are:

zip: a zip archive
gztar: a gzip compressed tar archive
bztar: a bzip2 compressed tar archive
xztar: an xz compressed tar archive

