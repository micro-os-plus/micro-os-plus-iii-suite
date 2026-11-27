# micro-os-plus-iii-suite

This repository provides the **µOS++ III Suite**, a set of projects that together
form the **third edition** of µOS++.

Each project within the suite is maintained in its own Git repository and is
included here as a submodule for unified access.

Compared to a monorepo, this approach enables more effective testing and
supports the development of distinct web sub-sites, each with its own URL.

## Getting the sources

The project is open source and is available on GitHub at
[micro-os-plus/micro-os-plus-iii-suite](https://github.com/micro-os-plus/micro-os-plus-iii-suite.git).

To obtain a local copy, execute:

```sh
rm -rf ~/Work/micro-os-plus/micro-os-plus-iii-suite.git && \
mkdir -p ~/Work/micro-os-plus && \
git clone --recurse-submodules \
  https://github.com/micro-os-plus/micro-os-plus-iii-suite.git \
  ~/Work/micro-os-plus/micro-os-plus-iii-suite.git
```

## Navigating the sources

The projects are grouped by role in a few top-level folders; the
leaf folders listed below are Git submodules, each pointing to the repository
of a separate project. All submodules track the `xpack-development` branch.

- `rtos/` - the µOS++ III real-time operating system
  ([micro-os-plus-iii](https://github.com/micro-os-plus/micro-os-plus-iii.git))
- `targets/` - the code specific to the hardware:
  - `architectures/` - the architecture ports:
    - `cortexm` - [micro-os-plus-iii-cortexm](https://github.com/micro-os-plus/micro-os-plus-iii-cortexm.git)
    - `aarch32` - [micro-os-plus-iii-aarch32](https://github.com/micro-os-plus/micro-os-plus-iii-aarch32.git)
    - `aarch64` - [micro-os-plus-iii-aarch64](https://github.com/micro-os-plus/micro-os-plus-iii-aarch64.git)
    - `riscv` - [micro-os-plus-iii-riscv](https://github.com/micro-os-plus/micro-os-plus-iii-riscv.git)
    - `synthetic-posix` - [micro-os-plus-iii-posix-arch](https://github.com/micro-os-plus/micro-os-plus-iii-posix-arch.git),
      to run on the development host
- `3rd-party/` - the third-party projects used by the tests, packaged as
  xPacks:
  - `arm/arm-cmsis-rtos-validator` - [arm-cmsis-rtos-validator-xpack](https://github.com/xpack-3rd-party/arm-cmsis-rtos-validator-xpack.git)
  - `chan/chan-fatfs` - [chan-fatfs-xpack](https://github.com/xpack-3rd-party/chan-fatfs-xpack.git)
  - `segger/segger-rtt` - [segger-rtt-xpack](https://github.com/xpack-3rd-party/segger-rtt-xpack.git)
  - `segger/segger-system-view` - [segger-system-view-xpack](https://github.com/xpack-3rd-party/segger-system-view-xpack.git)
- `maintenance/` - the files used to build, format and maintain the projects,
  not part of the µOS++ runtime

More projects will be added to the suite later.

## Using in projects

This hierarchy is useful to get all projects in a single place for inspecting or for
global searches, but for actual use in projects, the websites and tests are useless
and increase the clutter.

For actual use in projects it is more efficient to install the required projects
with `xpm`, which brings in only the sources and the CMake and Meson build
configurations.

The command below is a bulk-install example to show how the packages are named;
it is not a recommended dependency list for a single project. In a real project,
install the RTOS package and only one architecture package. The
architecture packages are alternatives, not components to combine in one
project.

```bash
mkdir my-project && cd my-project
xpm init
xpm install --verbose \
  github:micro-os-plus/micro-os-plus-iii \
  github:micro-os-plus/micro-os-plus-iii-cortexm \
  github:micro-os-plus/micro-os-plus-iii-aarch32 \
  github:micro-os-plus/micro-os-plus-iii-aarch64 \
  github:micro-os-plus/micro-os-plus-iii-riscv \
  github:micro-os-plus/micro-os-plus-iii-posix-arch
```

This command installs all the listed packages below the `xpacks` folder.

```bash
$ tree -L 3
.
├── LICENSE
├── package.json
└── xpacks
    └── @micro-os-plus
        ├── micro-os-plus-iii -> /home/ilg/.local/xPacks/@micro-os-plus/micro-os-plus-iii/7.0.1
        ├── micro-os-plus-iii-aarch32 -> /home/ilg/.local/xPacks/@micro-os-plus/micro-os-plus-iii-aarch32/0.1.0
        ├── micro-os-plus-iii-aarch64 -> /home/ilg/.local/xPacks/@micro-os-plus/micro-os-plus-iii-aarch64/0.1.0
        ├── micro-os-plus-iii-cortexm -> /home/ilg/.local/xPacks/@micro-os-plus/micro-os-plus-iii-cortexm/1.1.0
        ├── micro-os-plus-iii-posix-arch -> /home/ilg/.local/xPacks/@micro-os-plus/micro-os-plus-iii-posix-arch/1.0.1
        └── micro-os-plus-iii-riscv -> /home/ilg/.local/xPacks/@micro-os-plus/micro-os-plus-iii-riscv/0.1.0

9 directories, 2 files
```

By default, `xpm` links packages from its local cache into the `xpacks` folder.
To install copies in the project instead, use the `--copy` option.

(to be continued)
