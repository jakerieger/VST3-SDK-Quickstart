### 1. Downloading & Install the SDK

Download the official SDK from here: https://www.steinberg.net/vst3sdk.

Once downloaded, extract the `VST_SDK` folder contained inside to your `C:` drive root. The full path should be `C:\VST_SDK` once extracted.

Head in to the `vst3sdk` folder inside, then go to `bin` and then `Windows_x64`. Extract and install the `VST3PluginTestHost` application as this is what will be used to test and debug your plugin during development. Once installed, the plugin test host will be located at: 

```
C:\Program Files\Steinberg\VST3PluginTestHost\VST3PluginTestHost.exe
```

Once the test host has been installed, head back to the root `VST_SDK` folder and remove the `vst3sdk` folder as we will be cloning and using the sdk repository hosted on Steinberg's GitHub instead.

Open a terminal in the `VST_SDK` root and clone the `vst3sdk` repository from GitHub:

```sh
cd C:\VST_SDK
git clone --recursive https://github.com/steinbergmedia/vst3sdk.git
```

Once the repository has been cloned, `cd` into the `vst3sdk` folder and checkout the following tag:

```sh
cd vst3sdk
# checkout previous version, 3.7.11 has a memory leak bug in the plugin factory class
git checkout tags/v3.7.10_build_14
# update the repo to the checked out version
git pull origin master
```

### 2. Clone Template Project

After you've installed the SDK, clone the template project:

```sh
mkdir C:\VST_SDK\Projects
cd C:\VST_SDK\Projects
git clone https://github.com/jakerieger/ARKGain.git
```

The template project implements a simple, gui-less gain plugin. It implements all the required interfaces and classes and is a bare-bones CMake project that can be developed with any environment. It also implements processing support for both mono and stereo inputs/outputs.

### 3. Configure & Build Template Project

In order to build the template project, the same process used for any other CMake project is used.

#### Configure

```sh
cd C:\VST_SDK\Projects\ARKGain
mkdir build
cd build
cmake .. -G "Visual Studio 17 2022" -A x64 # MSVC compiler with x64 support
```

#### Build

```
cmake -- build .
```

This will output the final plugin in the `build\Debug\VST3` path, and will also create a symlink in `C:\Users\<username>\AppData\Local\Programs\Common\VST3`.

## Debugging

In order to debug a VST3 plugin, the `VST3PluginTestHost` application can be used. Simply set it as the target executable and with the symlink the build chain creates, your plugin should automatically appear in the plugin list inside the test host.