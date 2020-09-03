# libhdbpp-timescale

[![TangoControls](https://img.shields.io/badge/-Tango--Controls-7ABB45.svg?style=flat&logo=%20data%3Aimage%2Fpng%3Bbase64%2CiVBORw0KGgoAAAANSUhEUgAAACAAAAAkCAYAAADo6zjiAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAALEwAACxMBAJqcGAAAAsFJREFUWIXtl01IFVEYht9zU%2FvTqOxShLowlOgHykWUGEjUKqiocB1FQURB0KJaRdGiaFM7gzZRLWpTq2olhNQyCtpYCP1gNyIoUTFNnxZzRs8dzvw4Q6564XLnfOf73vedc2a%2BmZEKALgHrC3CUUR8CxZFeEoFalsdM4uLmMgFoIlZLJp3A9ZE4S2oKehhlaR1BTnyg2ocnW%2FxsxEDhbYij4EPVncaeASMAavnS%2FwA8NMaqACNQCew3f4as3KZOYh2SuqTVJeQNiFpn6QGSRVjTH9W%2FiThvcCn6H6n4BvQDvQWFT%2BSIDIFDAKfE3KOAQeBfB0XGPeQvgE67P8ZoB44DvTHmFgJdOQRv%2BUjc%2BavA9siNTWemgfA3TwGquCZ3w8szFIL1ALngIZorndvgJOR0GlP2gtJkzH%2Bd0fGFxW07NqY%2FCrx5QRXcYjbCbmxF1dkBSbi8kpACah3Yi2Sys74cVyxMWY6bk5BTwgRe%2BYlSzLmxNpU3aBeJogk4XWWpJKUeiap3RJYCpQj4QWZDQCuyIAk19Auj%2BAFYGZZjTGjksaBESB8P9iaxUBIaJzjZcCQcwHdj%2BS2Al0xPOeBYYKHk4vfmQ3Y8YkIwRUb7wQGU7j2ePrA1URx93ayd8UpD8klyPbSQfCOMIO05MbI%2BDvwBbjsMdGTwlX21AAMZzEerkaI9zFkP4AeYCPBg6gNuEb6I%2FthFgN1KSQupqzoRELOSed4DGiJala1UmOMr2U%2Bl%2FTWEy9Japa%2Fy41IWi%2FJ3d4%2FkkaAw0Bz3AocArqApwTvet3O3GbgV8qqjAM7bf4N4KMztwTodcYVyelywKSCD5V3xphNXoezuTskNSl4bgxJ6jPGVJJqbN0aSV%2Bd0M0aO7FCs19Jo2lExphXaTkxdRVgQFK7DZVDZ8%2BcpdmQh3wuILh7ut3AEyt%2B51%2BL%2F0cUfwFOX0t0StltmQAAAABJRU5ErkJggg%3D%3D)](http://www.tango-controls.org) [![License: GPL v3](https://img.shields.io/badge/License-GPL%20v3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0) [![](https://img.shields.io/github/release/tango-controls-hdbpp/libhdbpp-timescale.svg)](https://github.com/tango-controls-hdbpp/libhdbpp-timescale/releases)

- [libhdbpp-timescale](#libhdbpp-timescale)
  - [v0.9.0 To v0.10.0 Update](#v090-to-v0100-update)
  - [Cloning](#cloning)
  - [Bug Reports + Feature Requests](#bug-reports--feature-requests)
  - [Documentation](#documentation)
  - [Building](#building)
    - [Building Process](#building-process)
      - [Ubuntu](#ubuntu)
    - [Running tests and benchmarks](#running-tests-and-benchmarks)
  - [Installing](#installing)
    - [System Dependencies](#system-dependencies)
    - [Installation](#installation)
  - [License](#license)

HDB++ backend library for the TimescaleDb extenstion to Postgresql. This library is used by events subscribers to archive events from a Tango Controls system.

The library requires a correctly configured [TimescaleDb](https://www.timescale.com/) installation. 

The library has been build against a number of other projects, these have been integrated into the repository as sub modules. This may change at a later date if the build system is improved. Current sub modules are:

* spdlog - Logging system
* Catch2 - Unit test subsystem

## v0.9.0 To v0.10.0 Update 

This revision changes how both scalar and spectrum strings are stored. In 0.9.0 strings were escaped and quoted before being stored in the database. This had the effect that when the strings were retrieved they were still escaped/quoted. For consistency scalar strings were stored escaped/quoted.

To fix this, spectrum's of strings are now stored via insert strings using both the ARRAY syntax and dollar escape method. This means when they are retrieved frm the database they are longer escaped/quoted. To match this, scalar strings are also no longer stored escaped/quoted.

## Cloning

Currently this project is configured to express its dependencies as submodules. This may change in future if there is time to explore, for example, the Meson build system. To successfully clone the project and all its dependencies use the following:

```bash
git clone --recurse-submodules https://github.com/tango-controls-hdbpp/libhdbpp-timescale.git
```

## Bug Reports + Feature Requests

Please file the bug reports and feature requests in the issue tracker

## Documentation

* See the [doc](doc/README.md) directory for documentation on various topics, including building and installing.
* General documentations on the Tango HDB system can be found [here](http://tango-controls.readthedocs.io/en/latest/administration/services/hdbpp/index.html#hdb-an-archiving-historian-service)

## Building

For a full reference on building please refer to the [documentation](doc/build.md). This is a small summary of the standard building steps.

### Building Process

To compile this library, first ensure it has been recursively cloned so all submodules are present in /thirdparty. The build system uses pkg-config to find some dependencies, for example Tango. If Tango is not installed to a standard location, set PKG_CONFIG_PATH, i.e.

```bash
export PKG_CONFIG_PATH=/non/standard/tango/install/location
```

Then to build just the library:

```bash
mkdir -p build
cd build
cmake ..
make
```

The pkg-config path can also be set with the cmake argument CMAKE_PREFIX_PATH. This can be set on the command line at configuration time, i.e.:

```bash
...
cmake -DCMAKE_PREFIX_PATH=/non/standard/tango/install/location ..
...
```

#### Ubuntu

When using Postgres from the Ubuntu repositoris, it appears to install its development libraries in a slightly different location. Some info on this issue [here](https://gitlab.kitware.com/cmake/cmake/issues/17223). In this case, we set the PostgreSQL_TYPE_INCLUDE_DIR variable directly when calling cmake:

```
cmake -DPostgreSQL_TYPE_INCLUDE_DIR=/usr/include/postgresql/ ..
```

This should replace the call to cmake in the previous section.

### Running tests and benchmark

Please refer to the full [documentation](doc/build.md).

## Installing

For a full reference on installation please refer to the [documentation](doc/install.md). This is a small summary of the standard installation steps.
All submodules are combined into the final library for ease of deployment. This means just the libhdbpp-timescale.so binary needs deploying to the target system.

### System Dependencies

The running system requires libpq5 installed to support the calls Postgresql. On Debian/Ubuntu this can be deployed as follows:

```bash
sudo apt-get install libpq5
```

### Installation

After the build has completed, simply run:

```
sudo make install
```

The shared library will be installed to /usr/local/lib on Debian/Ubuntu systems.

## License

The source code is released under the LGPL3 license and a copy of this license is provided with the code.
