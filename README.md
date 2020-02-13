
# OSM Database Tools

Tools for creating replication feeds from the main OSM database.

**These tools are only useful if you run an OSM database like the main central
OSM database!**


## Prerequisites

You need a C++11 compliant compiler. GCC 8 and later as well as clang 7 and
later are known to work.

You also need the following libraries:

    Libosmium (>= 2.15.0)
        https://osmcode.org/libosmium
        Debian/Ubuntu: libosmium2-dev
        Fedora/CentOS: libosmium-devel

    Protozero (>= 1.6.3)
        https://github.com/mapbox/protozero
        Debian/Ubuntu: libprotozero-dev
        Fedora/CentOS: protozero-devel

    boost-program-options (>= 1.55)
        https://www.boost.org/doc/libs/1_55_0/doc/html/program_options.html
        Debian/Ubuntu: libboost-program-options-dev
        Fedora/CentOS: boost-devel
        openSUSE: boost-devel (use 'libboost_program_options-devel' for modern OS versions)

    bz2lib
        http://www.bzip.org/
        Debian/Ubuntu: libbz2-dev
        Fedora/CentOS: bzip2-devel
        openSUSE: libbz2-devel

    zlib
        https://www.zlib.net/
        Debian/Ubuntu: zlib1g-dev
        Fedora/CentOS: zlib-devel
        openSUSE: zlib-devel

    Expat
        https://libexpat.github.io/
        Debian/Ubuntu: libexpat1-dev
        Fedora/CentOS: expat-devel
        openSUSE: libexpat-devel

    cmake
        https://cmake.org/
        Debian/Ubuntu: cmake
        Fedora/CentOS: cmake
        openSUSE: cmake

    yaml-cpp
        https://github.com/jbeder/yaml-cpp
        Debian/Ubuntu: libyaml-cpp-dev

    libpqxx (version 6)
        https://github.com/jtv/libpqxx/
        Debian/Ubuntu: libpqxx-dev

    Pandoc
        (Needed to build documentation, optional)
        https://pandoc.org/
        Debian/Ubuntu: pandoc
        Fedora/CentOS: pandoc
        openSUSE: pandoc

On Linux systems most of these libraries are available through your package
manager, see the list above for the names of the packages. But make sure to
check the versions. If the packaged version available is not new enough, you'll
have to install from source. Most likely this is the case for Protozero and
Libosmium.

On macOS many of the libraries above will be available through Homebrew.


## Building

Use CMake to build in the usual way, for instance:

```
mkdir build
cd build
cmake ..
cmake --build .
```

## Database Setup

You need a PostgreSQL database with a user with REPLICATION attribute and a
database where this user has access containing an OSM database. There is an
(inofficial) `structure.sql` provided in this repository to set up such a
database.

You need to have the osm-logical plugin from
https://github.com/joto/osm-logical built and put into the plugin search path
of PostgreSQL.

## Running

There are several commands in the `build/src` directory. They all can be
called with `-h` or `--help` to see how they are run. They all need a common
config file, a template is in `osmdbt_config.yaml`. This will be found
automatically if it is in the local directory, use `-c` or `--config` to
set a different path.

### osmdbt-testdb

Test that the database connection works.

### osmdbt-enable-replication

Enable replication.

### osmdbt-disable-replication

Disable replication again.

### osmdbt-get-log

Get recent changes from the database. Creates a log file in an internal format
which can be read by `osmdbt-create-diff`. Also reports a LSN (Log Sequence
Number) which can be used with `osmdbt-catchup` to mark those changes as done.

### osmdbt-catchup

Mark changes up to the specified LSN as done.

### osmdbt-create-diff

Convert changes from the internal log format into an OSM change file. It will
be created with the same name as the log file, but the suffix `.osc.gz`.

## Tests

Call `ctest` in the build directory to run the tests after build.


## License

Copyright (C) 2020  Jochen Topf (jochen@topf.org)

This program is available under the GNU GENERAL PUBLIC LICENSE Version 3.
See the file LICENSE.txt for the complete text of the license.


## Authors

This program was written and is maintained by Jochen Topf (jochen@topf.org).

