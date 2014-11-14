Project Baker - Asset Catalog Generator
=============

**Generate Asset Catalog Images from source files to use with Baker Framework**
<http://bakerframework.com>



INSTALLATION
------------

First, clone this repository and step in:

    git clone git@github.com:tobiasstrebitzer/baker-asset-catalog-generator.git
    cd baker-asset-catalog-generator/


PREPARATION
-----------

First, modify AppIcon.png and LaunchImage.png to suit your needs. Make sure to leave some space all sides as the generator will crop the images (cropped areas are indicated as guides in the .psd files)

USAGE
-----

To generate all image sets, simply run:

    rake

To generate a specific image set, run:

    rake generate[AppIcon]
    # or
    rake generate[LaunchImage]


BUGS AND FEEDBACK
-----------------

* Submit your bugs here: <http://github.com/tobiasstrebitzer/baker-asset-catalog-generator/issues>
* Give us your feedback at: <office@bakerframework.com>
* Follow us on Twitter: <http://twitter.com/BakerFramework>



CHANGELOG
---------

* **1.0**
  * Added support AppIcon and LaunchImage, for iOS versions 5 through 8

LICENSE
-------

  _Copyright (C) 2014, Tobias Strebitzer_
  _Licensed under **BSD Opensource License** (free for personal and commercial use)_
