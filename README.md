Laird WB source HowTo
=====================

Preparation
-----------

To use to pull and build a specific project or version, you first need the program Repo from the Android project. You can get it by following the instructions here:

https://source.android.com/source/downloading.html#installing-repo

Getting the WB project
----------------------

First step, pick which release you want. Odds are you want the most recent of your release. Below shows how to "map" a release number to a GA release:

* __GA2__ : 3.4.x.x
* __GA3__ : 3.5.0.x
* __GA4__ : 3.5.1.x

Next, use Repo to initalize and fetch your release. This is a two-step process: first you tell Repo which manifest to use and then you tell it to fetch everything.

    mkdir wb45n_3.5.0.33_source
    cd wb45n_3.5.0.33_source
    repo init -u git@github.com:LairdCP/wb-manifests.git -m wb45n_3.5.0.33.xml
    repo sync

_Note: Repo will initialize a .repo directory and then place all files directly in the directory that you are in when you run the `repo` command. So we recommend making a subdirectory and working in there._

Building the WB project
-----------------------

There's more to go into about this, but for the sake of quick-start:

    cd wb
    make wb45n

Build sources archive
---------------------

We maintain a buildroot source binary archive on GitHub. Sometimes the source of the packages goes missing temporarily, or have been moved since last we updated the package in buildroot. Additionally, the Laird MSD packages our builds depend on are also in the repository.

If cloned, and your `BUILDROOT_DL_DIR` is set to this directory, then a build won't have to go and download the source. Alternately, any missing packages you can't find can be directly downloaded from GitHub and added to your `BUILDROOT_DL_DIR`.

GitHub:

    https://github.com/LairdCP/wb-package-archive

Clone:

	git clone git@github.com:LairdCP/wb-package-archive.git

Add to environment:

    export BUILDROOT_DL_DIR=/home/derosier/projects/wb-package-archive

Known source issues
-------------------

### Can't find the MSD package ###

__Applies to:__ All versions

The MSD package needs to be downloaded from the Laird website and placed where Buildroot can find it. If you use a `BUILDROOT_DL_DIR` place the file there.


### Can't find crda source package ###

__Applies to:__ GA2, GA3

The crda source package was moved since our last Buildroot update. We've since adjusted our code for future releases, but old releases will be unable to find the package. You can either copy the package from our package archive or apply the patch.

A. Copy:

     cd $BUILDROOT_DL_DIR
     wget https://github.com/LairdCP/wb-package-archive/blob/master/crda-1.1.3.tar.bz2?raw=true

B. Patch:

    diff --git a/package/crda/crda.mk b/package/crda/crda.mk
    index 56529da..040331a 100644
    --- a/package/crda/crda.mk
    +++ b/package/crda/crda.mk
    @@ -6,7 +6,7 @@

     CRDA_VERSION = 1.1.3
     CRDA_SOURCE = crda-$(CRDA_VERSION).tar.bz2
    -CRDA_SITE = http://wireless.kernel.org/download/crda
    +CRDA_SITE = https://www.kernel.org/pub/software/network/crda
     CRDA_DEPENDENCIES = host-pkgconf libnl openssl wireless-regdb
     CRDA_LICENSE = ISC
     CRDA_LICENSE_FILES = LICENSE

### Build error on wget ###

__Applies to:__ GA2, GA3, GA4 on newer build hosts

wget has a bug where it won't build on platforms with newer perl pod2man. More details can be found here: http://lists.gnu.org/archive/html/bug-wget/2013-06/msg00008.html

It was found that it's possible to pass through the error by simply reissuing the build command. Alternately, upgrading the wget package to a newer version can fix the problem. Change the `WGET_VERSION = 1.14` in `wb/buildroot/package/wget/wget.mk` file to version 1.15 to fix.

Known affected build hosts:

* Ubuntu 14.04

Known good build hosts:

* Ubuntu 12.04 LTS
* Ubunut 13.10


