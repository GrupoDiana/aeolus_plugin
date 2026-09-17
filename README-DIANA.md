# Aeolus Multibus

## 1. Requirements

To build and test the plugin, the following software is required:

* Visual Studio 2022
* CMake support for Visual Studio
* CMake
* Reaper for testing the VST3 plugin

## 2. Building the plugin

All commands below must be executed from the **root directory of the project**, where the `CMakeLists.txt` file is located.

### 2.1. Remove the previous build directory

Open a PowerShell terminal and run:

```powershell
Remove-Item -Recurse -Force .\out\build\x64-Debug
```

This removes the previous CMake build directory so that the project can be generated again using the current configuration.

### 2.2. Generate the Visual Studio project

Run:

```powershell
cmake -S . -B out\build\x64-Debug -G "Visual Studio 17 2022" -A x64
```

### 2.3. Build the project

Run:

```powershell
cmake --build out\build\x64-Debug --config Debug
```

If the build completes successfully, the VST3 plugin will be generated in:

```text
out\build\x64-Debug\Aeolus-multibus_artefacts\Debug\VST3
```

### 2.4. Note about Visual Studio and JUCE

Aeolus Multibus uses **8 independent audio outputs** through a multibus configuration.

The standard JUCE Standalone configuration is not initially designed to handle this multibus setup. Therefore, when running the project from Visual Studio, JUCE may trigger `jassert` checks related to the audio output configuration.

This does **not necessarily indicate that the VST3 plugin is invalid**. The 8 audio outputs should be tested from a plugin host such as Reaper.

## 3. Installing the plugin in Reaper

After building the project:

1. Go to:

```text
out\build\x64-Debug\Aeolus-multibus_artefacts\Debug\VST3
```

2. Locate the generated VST3 plugin.

3. Copy the generated plugin files/folder to a VST3 plugin directory. The default Windows VST3 directory is usually:

```text
C:\Program Files\Common Files\VST3
```

A separate folder can be created inside the VST3 directory to identify the plugin.

> Keep the generated files/folder structure unchanged.

## 4. Detecting the plugin in Reaper

Open Reaper and rescan the VST3 plugins:

**Options → Preferences → Plug-ins → VST → Re-scan**

If the plugin does not appear, check that the directory where the plugin was copied is included in Reaper's VST3 search paths.

## 5. Checking the audio outputs

This version of Aeolus provides **8 independent audio outputs**.

The plugin interface does not display the output configuration directly, so the outputs should be checked from the host, such as Reaper.

In Reaper, verify that the plugin exposes the different outputs and that each output can be routed independently.
