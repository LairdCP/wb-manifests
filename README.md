Laird WB source HowTo
=====================

Preparation
-----------

To use to pull and build a specific project or version, you first need the program Repo from the Android project. You can get it by following the instructions here:

https://source.android.com/source/downloading.html#installing-repo

Getting the WB project
----------------------

First step, pick which release you want. Odds are you want the most recent of your release. Below shows how to "map" a release number to a GA release:

* __GA4__ : 3.4.x.x
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

