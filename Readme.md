Heroku buildpack: Python
========================

This is a [Heroku buildpack](http://devcenter.heroku.com/articles/buildpacks) for Python apps.
It uses [virtualenv](http://www.virtualenv.org/) and [pip](http://www.pip-installer.org/).

[![Build Status](https://secure.travis-ci.org/heroku/heroku-buildpack-python.png?branch=master)](http://travis-ci.org/heroku/heroku-buildpack-python)

Usage
-----

Example usage:

    $ heroku config:set BUILDPACK_URL=https://github.com/bobwei/heroku-buildpack-python-for-geodjango.git

    $ git push heroku master

    ...
    -----> Heroku receiving push
    -----> Fetching custom buildpack... done
    -----> Python app detected
    -----> Fetching and installing GEOS 3.3.2
    -----> Installing ...
       GEOS installed
    -----> Fetching and installing Proj.4 4.7.0
    -----> Installing ...
       Proj.4 installed
    -----> Fetching and installing GDAL 1.8.1
    -----> Installing ...
       GDAL installed
    -----> Preparing virtualenv version 1.7
    ... etc.

You can also add it to upcoming builds of an existing application:

    $ heroku config:add BUILDPACK_URL=git://github.com/heroku/heroku-buildpack-python.git

The buildpack will detect your app as Python if it has the file `requirements.txt` in the root. It will detect your app as Python/Django if there is an additional `settings.py` in a project subdirectory.

It will use virtualenv and pip to install your dependencies, vendoring a copy of the Python runtime into your slug.  The `bin/`, `include/` and `lib/` directories will be cached between builds to allow for faster pip install time.

Hacking
-------

To use this buildpack, fork it on Github.  Push up changes to your fork, then create a test app with `--buildpack <your-github-url>` and push to it.

To change the vendored virtualenv, unpack the desired version to the `src/` folder, and update the virtualenv() function in `bin/compile` to prepend the virtualenv module directory to the path. The virtualenv release vendors its own versions of pip and setuptools.

Notes
-------

All libraries are stored in /app/.github.

IMPORTANT: You will need to set two Django settings in order for GEOS and GDAL to work properly!

GEOS_LIBRARY_PATH = '/app/.geodjango/geos/lib/libgeos_c.so'

GDAL_LIBRARY_PATH = '/app/.geodjango/gdal/lib/libgdal.so'