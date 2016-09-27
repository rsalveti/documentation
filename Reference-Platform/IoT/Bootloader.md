# Bootloader

Mynewt's bootloader is used, which provides support for signature checking and the dual bank architecture.

## Setting up the development environment

The **newt** tool is required in order to build the bootloader. Follow the install process as described at http://mynewt.apache.org/latest//newt/install/newt_linux/

With newt installed, create the base project that will be used for the specific targets:
```
$ newt new bootloader
$ cd bootloader
$ newt install -v
```

Since there is no upstream support yet for Carbon, switch to the git tree hosted at github.com/linaro:
```
$ cd bootloader/repos/apache-mynewt-core
$ git remote add linaro https://github.com/Linaro/incubator-mynewt-core.git
$ git fetch linaro
$ git checkout linaro/dualbank_rsa_enabled_bootloader
```

## Carbon

### Building bootloader for Carbon

Create the target:
```
$ cd bootloader
$ newt target create bootloader_carbon
$ newt target set bootloader_carbon app=@apache-mynewt-core/apps/boot
$ newt target set bootloader_carbon bsp=@apache-mynewt-core/hw/bsp/96b-carbon
$ newt target set bootloader_carbon build_profile=optimized
$ newt target show
targets/bootloader_carbon
    app=@apache-mynewt-core/apps/boot
    bsp=@apache-mynewt-core/hw/bsp/96b-carbon
    build_profile=optimized
targets/my_blinky_sim
    app=apps/blinky
    bsp=@apache-mynewt-core/hw/bsp/native
    build_profile=debug
```

Build the bootloader:
```
$ newt build bootloader_carbon
```

The bootloader application binary can be found at ***bin/bootloader_carbon/apps/boot/boot.elf.bin***.

### Flashing the bootloader

```
$ sudo dfu-util -d [0483:df11] -a 0 -D boot.elf.bin -s 0x08000000
```

## Nitrogen

### Building bootloader for Nitrogen

Create the target:
```
$ cd bootloader
$ newt target create bootloader_nitrogen
$ newt target set bootloader_nitrogen app=@apache-mynewt-core/apps/boot
$ newt target set bootloader_nitrogen bsp=@apache-mynewt-core/hw/bsp/nrf52dk
$ newt target set bootloader_nitrogen build_profile=optimized
$ newt target show
targets/bootloader_nitrogen
    app=@apache-mynewt-core/apps/boot
    bsp=@apache-mynewt-core/hw/bsp/nrf52dk
    build_profile=optimized
targets/my_blinky_sim
    app=apps/blinky
    bsp=@apache-mynewt-core/hw/bsp/native
    build_profile=debug
```

Build the bootloader:
```
$ newt build bootloader_nitrogen
```

The bootloader application binary can be found at ***bin/bootloader_nitrogen/apps/boot/boot.elf.bin***.

### Flashing the bootloader

```
$ pyocd-flashtool -d debug -t nrf52 -a 0x0 boot.elf.bin
```
