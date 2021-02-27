# Universal Offset Porter

This application allows you to port an offset from a source memory dump to one or more destination memory dump(s). Alternatively, you can also just find a working array of byte (= AOB) sequence with an offset and a match count which can be used to find the source offset and destination offsets alike.

Porting is done via array of byte (= AOB) searching or also known as pattern searching. Searching for the AOB template, taking the first matching address and adding the AOB template offset brings you directly to the ported offset.

## Requirements

### Windows

This application may require you to install the `Microsoft Visual C++ Redistributable for Visual Studio 2015, 2017 and 2019` from [here](https://support.microsoft.com/en-us/topic/the-latest-supported-visual-c-downloads-2647da03-1eea-4433-9aff-95f26a218cc0):

```
>dumpbin /dependents UniversalOffsetPorter.exe
Microsoft (R) COFF/PE Dumper Version 14.28.29336.0
Copyright (C) Microsoft Corporation.  All rights reserved.


Dump of file UniversalOffsetPorter.exe

File Type: EXECUTABLE IMAGE

Image has the following dependencies:

    KERNEL32.dll
    USER32.dll
    MSVCP140.dll
    MSVCP140_ATOMIC_WAIT.dll
    VCRUNTIME140_1.dll
    VCRUNTIME140.dll
    api-ms-win-crt-runtime-l1-1-0.dll
    api-ms-win-crt-string-l1-1-0.dll
    api-ms-win-crt-stdio-l1-1-0.dll
    api-ms-win-crt-heap-l1-1-0.dll
    api-ms-win-crt-time-l1-1-0.dll
    api-ms-win-crt-convert-l1-1-0.dll
    api-ms-win-crt-filesystem-l1-1-0.dll
    api-ms-win-crt-environment-l1-1-0.dll
    api-ms-win-crt-locale-l1-1-0.dll
    api-ms-win-crt-math-l1-1-0.dll
```

### Linux

The application may require you to install [`Intel TBB`](https://software.intel.com/content/www/us/en/develop/tools/oneapi/components/onetbb.html) via e.g. `sudo apt install libtbb-dev`. The application has the following dependencies:

```
$ ldd UniversalOffsetPorter.elf
    linux-vdso.so.1 (0x00007ffe10128000)
    libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007fca4e4e0000)
    libpthread.so.0 => /lib/x86_64-linux-gnu/libpthread.so.0 (0x00007fca4e4bd000)
    libdl.so.2 => /lib/x86_64-linux-gnu/libdl.so.2 (0x00007fca4e4b7000)
    libtbb.so.2 => /usr/lib/x86_64-linux-gnu/libtbb.so.2 (0x00007fca4e471000)
    libstdc++.so.6 => /usr/lib/x86_64-linux-gnu/libstdc++.so.6 (0x00007fca4e290000)
    libgcc_s.so.1 => /lib/x86_64-linux-gnu/libgcc_s.so.1 (0x00007fca4e275000)
    /lib64/ld-linux-x86-64.so.2 (0x00007fca4e773000)
    libm.so.6 => /lib/x86_64-linux-gnu/libm.so.6 (0x00007fca4e124000)
```

## Repository Files

This repository contains the **README.md**, the **LICENSE**, the compiled **UniversalOffsetPorter.elf** (Linux version) and the **UniversalOffsetPorter.exe** (Windows version).

If there is a need for a Mac OS X version, I might work on compiling that one as well and add it to the repository.

## Usage

This is a command line application. You have to pass the respective information such as source memory dump file path and source offset to the application via your system's command line. Alternatively, there are more optional options to configure the offset porter. You can list them by passing the `--help` flag.

Command line parsing works via `CLI11` so you may want to consult the documentation [here](https://cliutils.github.io/CLI11/book/) if you have any questions on how to pass certain information to the application.

#### Note:

[You can write batch/shell scripts with your command and re-run them later](https://bullywiihacks.forumotion.com/t6494-#32192). This way you don't have to type all command line switches again if you want to repeat a port or modify its arguments.

## Bug Reports/Feature Requests

If you find a definite bug or a lacking feature, please create a **detailed** GitHub issue (e.g. with all input and output data) and I will try to help you out.

## Performance Tips
The most costly operation is scanning the memory dump(s) for a byte pattern, therefore you can try the following:
* Minimizing the size of the memory range to be scanned
* Minimizing the amount of search templates being used
* Having more cores/threads available in your PC as well as using a faster processor
* Not using placeholders (`??`) in the default value bytes since they cause additional comparison overhead

## Dealing with false results
It's possible that the offset porter produces false ports, there is no guarantee that every offset is ported correctly. It is your responsibility to double-check and make sure your ports are correct, preferably by testing. You can furthermore fine-tune the command line options to improve the automated porting results.

## Contributing
If you appreciate this tool, [feel free to donate any amount of money to me on PayPal](https://www.paypal.me/bullywiiplaza) to support further development and updates.
