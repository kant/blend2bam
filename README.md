![Pipeline](https://github.com/Moguri/blend2bam/workflows/Pipeline/badge.svg)
[![Python Versions](https://img.shields.io/pypi/pyversions/panda3d-blend2bam.svg)](https://pypi.org/project/panda3d-blend2bam/)
[![Panda3D Versions](https://img.shields.io/badge/panda3d-1.9%2C%201.10%2C%201.11-blue.svg)](https://www.panda3d.org/)
[![License](https://img.shields.io/github/license/Moguri/panda3d-blend2bam.svg)](https://choosealicense.com/licenses/mit/)


# blend2bam
`blend2bam` is a CLI tool to convert Blender blend files to Panda3D BAM files.
It also supplies a Python file loader to add "native" blend file support to Panda3D.


## Installation

Use [pip](https://github.com/panda3d/panda3d) to install the panda3d-blend2bam package:

```bash
pip install panda3d-blend2bam
```

`blend2bam` also requires that Blender is installed.
Ideally `blender` is available on the system PATH.
However, if it's not, the directory containing `blender` can be specified with `--blender-dir` (see CLI usage).

## Usage

### CLI

```
usage: blend2bam [-h] [--version] [-m {legacy,pbr}] [--physics-engine {builtin,bullet}] [--srcdir SRCDIR]
                 [--blender-dir BLENDER_DIR] [--append-ext] [--pipeline {gltf,egg}]
                 [--no-srgb] [--textures {ref,copy,embed}]
                 src [src ...] dst

CLI tool to convert Blender blend files to Panda3D BAM files

positional arguments:
  src                   source path
  dst                   destination path

optional arguments:
  -h, --help            show this help message and exit
  --version             show program's version number and exit
  -m {legacy,pbr}, --material-mode {legacy,pbr}
                        control how materials are exported
  --physics-engine {builtin,bullet}
                        the physics engine to build collision solids for
  --srcdir SRCDIR       a common source directory to use when specifying multiple source files
  --blender-dir BLENDER_DIR
                        directory that contains the blender binary
  --append-ext          append extension on the destination instead of replacing it (batch mode only)
  --pipeline {gltf,egg}
                        the backend pipeline used to convert files
  --no-srgb             do not load textures as sRGB textures (only for glTF pipelines)
  --textures {ref,copy,embed}
                        how to handle external textures
  --animations {embed,separate,skip}
                        how to handle animation data
```

### Python File Loader

`blend2bam` also supports Panda3D's Python file loader API (requires Panda3D 1.10.4+) to seamlessly adds blend file support to Panda3D's `Loader` classes.
This *does not* add support to `pview`, which is a C++ application that does not support Python file loaders.

## Pipelines

`blend2bam` has support for multiple backend "pipelines." Currently, `gltf` and `egg` are supported.
For Blender 2.7x, `gltf` uses [blendergltf](https://github.com/Kupoman/blendergltf) and [panda3d-gltf](https://github.com/Moguri/panda3d-gltf) while `egg` uses [YABEE](https://github.com/09th/YABEE) and `egg2bam` from the Panda3d SDK.
For Blender 2.80+ (including 2.90+), only `gltf` is supported and uses the glTF exporter built into Blender 2.80+ instead of blendergltf.
The below table hightlights some of the differences.

|Feature|glTF (2.7x)|EGG (2.7x)|glTF (2.8+)|
|---|:---:|:---:|:---:|
|Static Meshes|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|
|Textures|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|
|Multiple Diffuse Textures|:x:|:heavy_check_mark:|:x:|
|Legacy Materials|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:<sup>2</sup>|
|PBR Materials|:heavy_check_mark:|:x:|:heavy_check_mark:|
|Lights|:heavy_check_mark:|:x:|:heavy_check_mark:|
|Skinned Meshes|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|
|Skeletal Animations|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|
|Shape Keys|:x:|:heavy_check_mark:|:heavy_check_mark:|
|Shape Key Animations|:x:|:heavy_check_mark:|:heavy_check_mark:<sup>1</sup>|
|Object Animations|:x:|:x:|:x:|
|CollisionSolids|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|
|Bullet Shapes|:heavy_check_mark:|:x:|:heavy_check_mark:|
|Tags from Game Properties|:heavy_check_mark:|:heavy_check_mark:|:x:|
|Tags from Custom Properties|:heavy_check_mark:|:x:|:heavy_check_mark:|
|Convert Particle Systems to Meshes|:heavy_check_mark:|:x:|:heavy_check_mark:|
|Separate Animation Files|:heavy_check_mark:|:x:<sup>3</sup>|:heavy_check_mark:|

<sup>1</sup> Shape key animations require Panda3D 1.10.6.

<sup>2</sup> Only supports diffuse color (factor and map pulled from base color) and normal maps.
Requires `panda3d-gltf` >= 0.9.

<sup>3</sup> Supported by EGG, but not implemented in blend2bam

## Conversion Guides

The following conversion guides describe how Blender data gets converted to Panda3D data for the various pipelines blend2bam supports:

* [glTF for Blender 2.80+](docs/conversion-guide-gltf28.md)
* [glTF for Blender 2.79](docs/conversion-guide-gltf27.md)
* [EGG for Blender 2.79](docs/conversion-guide-egg27.md)

## License

[MIT](https://choosealicense.com/licenses/mit/)
