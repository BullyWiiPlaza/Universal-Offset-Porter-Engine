# Universal Offset Porter

This application allows you to port an offset from a source memory dump to one or more destination memory dump(s). Alternatively, you can also just find a working array of byte (= AOB) sequence with an offset and a match count which can be used to find the source offset and destination offsets alike.

Porting is done via array of byte (= AOB) searching or also known as pattern searching. Searching for the AOB template, taking the first matching address and adding the AOB template offset brings you directly to the ported offset.

### Use Cases
In the following, common use cases (and command line commands) will be posted/explained.

* Porting an offset from a source memory dump to a destination memory dump with multi-threading and a default value bytes pattern:  
`--memory-dump "[GM4E01] Mario Kart - Double Dash.raw" "[GM4P01] Mario Kart - Double Dash.raw" --offset 0x3BC1C0 --run-multi-threaded --default-value-bytes-pattern "00 00 00 00 00 00 00 00 00 00 00 41 41 41 00 00"`
* Finding exactly 4 working search templates for the memory dump's offset:
`--memory-dump "[GM4P01] Mario Kart - Double Dash.raw" --offset 0x3BC1C0 --search-template-count 4`

## Requirements

### Windows

This application may require you to install the `Microsoft Visual C++ Redistributable for Visual Studio 2015, 2017, 2019 and 2022` from [here](https://support.microsoft.com/en-us/topic/the-latest-supported-visual-c-downloads-2647da03-1eea-4433-9aff-95f26a218cc0):

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

### macOS

```
% oTool -L UniversalOffsetPorter.macho
UniversalOffsetPorter.macho:
        /usr/lib/libc++.1.dylib (compatibility version 1.0.0, current version 1300.23.0)
        /usr/lib/libSystem.B.dylib (compatibility version 1.0.0, current version 1311.100.3)
```

## Files

[The release](https://github.com/BullyWiiPlaza/Universal-Offset-Porter-Engine/releases) contains the **README.md**, the **LICENSE**, the compiled **UniversalOffsetPorter.elf** (Linux version), **UniversalOffsetPorter.macho** (MacOS X version) and the **UniversalOffsetPorter.exe** (Windows version) and the **DLLs** for the **Windows** version (if any).

## Usage

This is a command line application. You have to pass the respective information such as source memory dump file path and source offset to the application via your system's command line. Alternatively, there are more optional options to configure the offset porter. You can list them by passing the `--help` flag:

```
>UniversalOffsetPorter.exe --help
[2024-04-15 10:24:02.815] [5000] [info] Windows C++ Universal Offset Porter Engine v2.1
Release Build
Copyright (c) 2019 - 2024 BullyWiiPlaza Productions
[2024-04-15 10:24:02.817] [5000] [info] Parsing command line arguments...
Universal Offset Porter Engine
Usage: UniversalOffsetPorter.exe [OPTIONS]

Options:
  -h,--help                   Print this help message and exit
  --memory-dump TEXT ...      The memory dump(s) for porting the offset from/to
  --offset UINT:POSITIVE ...  The offset(s) to use for finding working search template(s)
  --excluded-destination-offsets UINT:POSITIVE ...
                              The destination offset(s) to exclude from the successfully ported offsets (for use to refine a porting process)
  --maximum-porting-try-count UINT:POSITIVE
                              The maximum amount of porting tries before giving up
  --minimum-search-template-length-range TEXT
                              The search template length range to use
  --maximum-allowed-byte-failures-count UINT:POSITIVE
                              The maximum amount of bytes which may differ in a search template to still be considered a successful match
  --entropy-threshold FLOAT:FLOAT in [0 - 1]
                              The entry to use to sort out search templates
  --maximum-search-template-offset UINT:POSITIVE
                              The maximum search template offset to accept
  --search-template-count UINT:POSITIVE
                              The amount of search templates to find
  --maximum-search-result-count UINT:POSITIVE
                              The maximum amount of matches a search template is allowed to find before being discarded
  --required-destination-offset-match-count INT:POSITIVE
                              The amount of search templates to find which reach the same destination offset
  --equal-default-value-byte-count UINT:POSITIVE
                              The amount of default value bytes which have to be the same in all memory dumps
  --search-template-iteration-step-size UINT:POSITIVE
                              The iteration step size for generating search templates
  --default-value-bytes-pattern TEXT
                              The default value byte pattern to use for matching default values on all memory dumps
  --string-search-algorithm TEXT:{Boyer-Moore,Boyer-Moore-Horspool,Default,Knuth-Morris-Pratt}
                              The string search algorithm to use
  --match-pattern-till-source-offset-only
                              Whether the pattern matcher will only consider matches before the source offset as relevant
  --destination-offset-range TEXT ...
                              The destination offset range(s) to use for the destination memory dump(s) respectively
  --run-multi-threaded        Whether to run the offset porter multi-threaded (faster but may produce less easily reproducible results)
  --thread-count INT:POSITIVE The (maximum) amount of threads to use (when processing multi-threaded)
  --verbose                   Whether to print verbose output
  --working-directory TEXT    The directory to use as a relative base for resolving file paths
  --output-file-path TEXT     The (JSON) file path to store ported offset results to
```

Command line parsing works via `CLI11` so you may want to consult the documentation [here](https://cliutils.github.io/CLI11/book) if you have any questions on how to pass certain information to the application.

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
