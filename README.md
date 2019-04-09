Firmware builder on Docker container
======================

This repository is intended to create a simple environment to generate builds of custom micropython firmware inside Odditive. However, you can also freely use it providing links to your repo as the arguments.

Usage
------------------

Build the docker image:

```bash
  docker build -t odditive-micropython .
```

Use `build-args` to provide arguments such as:

```bash
  docker build -t odditive-micropython --build-arg BRANCH=add-wps-and-netstatus .
```

Then create a container from the image using:

```bash
  docker create --name odditive-micropython odditive-micropython
```

Then copy the the firmware into your filesystem.

```bash
  docker cp odditive-micropython:/micropython/ports/esp32/build/firmware.bin firmware.bin
```

Deployment
------------------

Clear your ESP:

```bash
  esptool.py --chip esp32 --port /dev/tty.SLAB_USBtoUART erase_flash
```

Install firmware into the ESP:

```bash
  esptool.py --chip esp32 --port /dev/tty.SLAB_USBtoUART --baud 460800 write_flash -z 0x1000 firmware.bin
```

Configurations
------------------

Provided arguments are following:

`REPOSITORY` - Link to your fork of micropython.
`BRANCH` - Git branch of your fork to be deployed.
`VERSION` - Hash of supported ESP-IDF version.