# OpenGL-CMake-Template
### Windows 11 Setup Guide

- Install Visual Studio 2022 with C++ Development option, with Linux support selected (if you want to build for Linux using wsl2).

- Open VS 2022 Developer PowerShell (this is needed as env variables needed for cl.exe and other tools have been automatically set). You could also use environment that setups env variables needed for MSVC, e.g. selecting Visual Studio Toolchain in CLion, and running a CMake Preset through the GUI to
  generate needed build files.

```
git clone --recurse-submodules https://github.com/mem64-bits/OpenGL-CMake-Template

cd vcpkg
# Setups vcpkg executable for project (only needed once)
.\bootstrap-vcpkg.bat

#Select either "windows-msvc-debug" or "windows-msvc-release"
cmake --preset <windows-msvc-debug / windows-msvc-release>

# After the cmake build files have been generated and vcpkg packages
# have been installed the project can be built through the IDE
# "VS # 2022" or "Clion"

# To manually build run the preset "windows-msvc-debug" for the debug build

cmake --build windows-msvc-debug

# And run "windows-msvc-release" for the release build
cmake --build windows-msvc-release
```

## Install Guide for Linux

### Ubuntu / Debian

Ubuntu has the greatest support for vcpkg. Install any packages that vcpkg will ask for if needed.

```
# Ensure cmake and build tools are installed
sudo apt install cmake ninja-build build-essential
```

```
git clone --recurse-submodules https://github.com/mem64-bits/OpenGL-CMake-Template

cd vcpkg
# Setups vcpkg executable for project (only needed once)
./bootstrap-vcpkg.sh

#Select either "linux-g++-debug" or "linux-g++-release"
cmake --preset <linux-g++-debug / windows-msvc-linux-g++-release>

# After the cmake build files have been generated and vcpkg packages
# have been installed the project can be built manually or through
the IDE "Clion".

# To manually build run the preset "linux-g++-debug" for the debug build

cmake --build linux-g++-debug

# And run "linux-g++-release" for the release build
cmake --build linux-g++-release
```

### Fedora

Install a desktop environment with x11 / wayland support or
install these packages to ensure glfw works

```
sudo dnf group install "development-tools" ninja-build cmake

```

```
git clone --recurse-submodules https://github.com/mem64-bits/OpenGL-CMake-Template

cd vcpkg
# Setups vcpkg executable for project (only needed once)
./bootstrap-vcpkg.sh

#Select either "linux-g++-debug" or "linux-g++-release"
cmake --preset <linux-g++-debug / windows-msvc-linux-g++-release>

# After the cmake build files have been generated and vcpkg packages
# have been installed the project can be built manually or through
the IDE "Clion".

# To manually build run the preset "linux-g++-debug" for the debug build

cmake --build linux-g++-debug

# And run "linux-g++-release" for the release build
cmake --build linux-g++-release
```

### Arch

Install a desktop environment with x11 / wayland support or
install these packages to ensure glfw works

```
sudo pacman -Syu --needed base-devel cmake ninja
```

```
git clone --recurse-submodules https://github.com/mem64-bits/OpenGL-CMake-Template

cd vcpkg
# Setups vcpkg executable for project (only needed once)
./bootstrap-vcpkg.sh

#Select either "linux-g++-debug" or "linux-g++-release"
cmake --preset <linux-g++-debug / windows-msvc-linux-g++-release>

# After the cmake build files have been generated and vcpkg packages
# have been installed the project can be built manually or through
the IDE "Clion".

# To manually build run the preset "linux-g++-debug" for the debug build

cmake --build linux-g++-debug

# And run "linux-g++-release" for the release build
cmake --build linux-g++-release
```
