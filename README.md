[![Build Status](https://github.com/mavlink/mavlink/workflows/Test%20and%20deploy/badge.svg)](https://github.com/mavlink/mavlink/actions?query=branch%3Amaster)

# MAVLink

MAVLink -- Micro Air Vehicle Message Marshalling Library.

MAVLink is a very lightweight, header-only message library for communication between drones and/or ground control stations. It consists primarily of message-set specifications for different systems ("dialects") defined in XML files, and Python tools that convert these into appropriate source code for [supported languages](https://mavlink.io/en/#supported_languages). There are additional Python scripts providing examples and utilities for working with MAVLink data.

> **Tip** MAVLink is very well suited for applications with very limited communication bandwidth. Its reference implementation in C is highly optimized for resource-constrained systems with limited RAM and flash memory. It is field-proven and deployed in many products where it serves as interoperability interface between components of different manufacturers.


## Quick start

### Generate C headers

To install the minimal MAVLink environment on Ubuntu LTS 20.04 or 22.04, enter the following on a terminal:

```bash
# Dependencies
sudo apt install python3-pip

# Clone mavlink into the directory of your choice
git clone https://github.com/mavlink/mavlink.git --recursive
cd mavlink

python3 -m pip install -r pymavlink/requirements.txt
```

You can then build the MAVLink2 C-library for `message_definitions/v1.0/common.xml` from the `/mavlink` directory as shown:

```bash
python3 -m pymavlink.tools.mavgen --lang=C --wire-protocol=2.0 --output=generated/include/mavlink/v2.0 message_definitions/v1.0/common.xml
```

### Use from cmake

To include the headers in cmake, install them locally, e.g. into the directory `install`:

```
cmake -Bbuild -H. -DCMAKE_INSTALL_PREFIX=install -DMAVLINK_DIALECT=common -DMAVLINK_VERSION=2.0
cmake --build build --target install
```

Then use `find_package` to get the dependency in `CMakeLists.txt`:

```
find_package(MAVLink REQUIRED)

add_executable(my_program my_program.c)

target_link_libraries(my_program PRIVATE MAVLink::mavlink)
```

And pass the local install directory to cmake (adapt to your directory structure):

```
cd ../my_program
cmake -Bbuild -H. -DCMAKE_PREFIX_PATH=../mavlink/install
```

For a full example, check [examples/c](examples/c).

*Note: even though we use `target_link_libraries` in cmake, it doesn't actually "link" to MAVLink as it's just a header-only library.*

### Other instructions

Instructions for using the C libraries are then covered in [Using C MAVLink Libraries (mavgen)](https://mavlink.io/en/mavgen_c/).

> **Note:** [Installing the MAVLink Toolchain](https://mavlink.io/en/getting_started/installation.html) explains how to install MAVLink on other Ubuntu platforms and Windows, while [Generating MAVLink Libraries](https://mavlink.io/en/getting_started/generate_libraries.html) explains how to build MAVLink for the other programming languages [supported by the project](https://mavlink.io/en/#supported_languages).
> The sub-topics of [Using MAVLink Libraries](https://mavlink.io/en/getting_started/use_libraries.html) explain how to use the generated libraries.


## Key Links

* [Documentation/Website](https://mavlink.io/en/) (mavlink.io/en/)
* [Discussion/Support](https://mavlink.io/en/#support)
* [Contributing](https://mavlink.io/en/contributing/contributing.html)
* [License](https://mavlink.io/en/#license)


## git log (from master) 2025/8/12 12:03

commit b27d03bffd7969b30242a00aeaa6c0430d34d429 (HEAD -> master, origin/master, origin/HEAD)
Author: Mathieu Bresciani <brescianimathieu@gmail.com>
Date:   Wed Aug 6 10:33:33 2025 +0200

    Add GLOBAL_POSITION message (#2256)
    
    * Add EXTERNAL_GLOBAL_POSITION message
    
    * GLOBAL_POSITION: add velocity and heading fields
    
    * GLOBAL_POSITION: split h and v speed accuracies
    
    * Update message_definitions/v1.0/development.xml
    
    Co-authored-by: Hamish Willee <hamishwillee@gmail.com>
    
    * Update message_definitions/v1.0/development.xml
    
    * GLOBAL_POSITION: strip down to minimum required fields
    
    And add source field
    
    * GLOBAL_POSITION: add source enum
    
    Co-authored-by: Hamish Willee <hamishwillee@gmail.com>

# GUI

```
sudo apt install python3-tk

git clone XXX

sudo apt install python3-pip
```

* 用户自定义的消息都在message_definitions/v1里面，所有依赖于common.xml的都需要放到同级目录下

```
python -m mavgenerate   # GUI operation
```

# Command Line Tool 
* mavgen.py 是一个命令行工具,用于生成用于各种编程语言的 MAVLink 库。
您可以从 mavlink 目录运行 mavgen。但是,如果您在 mavlink 目录之外,则需要将 mavlink 目录添加到 PYTHONPATH 环境变量。
* Mavgen是Mavgenerate使用的后端。

```
python3 -m pymavlink.tools.mavgen --lang=C --wire-protocol=2.0 --output=generated/include/mavlink/v2.0 message_definitions/v1.0/your_custom_dialect.xml
```