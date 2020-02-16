Code frame structure
========



## Directory Introduction

| Directory | Subdirectory | Subdirectory2 | Subdirectory3 | Brief | 
| -- | -- | -- | -- | -- |
|  assets  |  |  | | Resource like image |
|  projects  |  | | | Project dir| 
|  tools | | | | tools |
| components|┐ | | | component |
|               | └-boards | | | code for board specific |
|               | └-drivers | | | driver code |
|               | └-micropython |┐ | |  Micropython related|
|               |                |└-core | | micropython source code |
|               |                |└-port|┐ | maixpy port code |
|               |                |          |└-builtin_py | maixpy defualt builtin script |
|               |                |          |└-include | headers |
|               |                |          |└-src | modules |
|               | └-spiffs | | | SPIFFS |
|               | └-utils | | | utils|





## Add code

The project is organized using `CMake`, and the project supports multiple configurable options (` Kconfig`)

* If you do not add folders and configuration items, you can add files to the existing folder for compilation.
* If you need to add modules, you can modify `CMakeLists.txt` to add content, you can refer to [c_cpp_project_framework] (https://github.com/Neutree/c_cpp_project_framework) with less content
* If you need to add configuration items, you can modify the `Kconfig` file to achieve the purpose. All configuration items will generate macro definitions and add them to` global_config.h` (generated file) at compile time, and in `CmakeLists.txt` This macro definition can be used in files.
> For example, `config BOARD_M5STICK` is defined in Kconfig, and CMakeLists.txt can determine whether to compile specific code by judging whether CONFIG_BOARD_M5STICK is true. When compiling, you can choose whether to check by `python3 project.py menuconfig`


