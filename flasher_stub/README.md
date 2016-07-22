This is the source of the flasher stub developed by Cesanta.

# Rebuilding the stub

The Makefile takes a few parameters:

* SDK_PATH - path to Espressif SDK root.
* CROSS - cross compiler prefix. Defaults to xtensa-lx106-elf-.
* WRAP_STUB - command to invoke wrap_stub.py. Default will work unless /usr/bin/python is not Python 2.x.

Build the stub by running make, ie:

``` shell
make SDK_PATH=/path/to/my/esp_iot_sdk_dir
```
The build process produces a JSON-formatted binary stub, `build/stub_flasher.json`. This can be tested standalone (see below). It also produces an equivalent `build/stub_flasher.json.py` which is preformatted as a Python string, ready to copy into esptool.py

# Testing

## Quick stub tests

Rather than copying the stub JSON into esptool.py each time, the `esptool_test_stub.py` program will inject the newly build stub into esptool.py and then pass through the same commands. It must be run from this directory.

ie:

``` shell
./esptool_test_stub.py write_flash 0 /path/to/my/binary.bin
```

## run_stub method

The `ESPROM.run_stub()` function in esptool.py has an argument `read_output` that will optionally print to the console any output that the stub produces. If set then this disables normal stub behaviour. This can be useful for outputting values from the stub for low-level debugging.

As it is a developer option, `read_output=True` is not currently available on the esptool.py command line. You can enable it temporarily by editing the `run_stub()` call inside esptool.py, or you can use a Python shell or small test program to instantiate an ESPROM object and then call the method directly.

