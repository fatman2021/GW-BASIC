# Microsoft GW-BASIC Interpreter Source Code

This repo contains the original source-code for Microsoft's GW-BASIC interpreter, as of 1983,
adjusted for assembling with JWasm or available versions of the Microsoft Macro Assembler.

## Announcement blog (from Microsoft)
https://devblogs.microsoft.com/commandline/microsoft-open-sources-gw-basic/

## Progress

### Assembling

All files can now be assembled with Microsoft MASM 5.1A.
This is the version that currently seems to match the code most closely.

It is now also possible to cross-assemble the source files ― with some
preprocessing ― using JWasm and JWlink.

### Implementation

The interpreter is semi-working, but some parts of the platform-specific
support code are still missing or incomplete.

Specifically, [Diomidis Spinellis](https://github.com/dspinellis/GW-BASIC)
had observed that several OEM-specific functions were missing from the
original source code release, and have to be added.
Most of these have been implemented in the new modules `OEM.ASM`,
`OEMEV.ASM`, and `OEMSND.ASM`.

However:
  * Some routines are still missing, and are currently stubs: `INICOM`, `RDPEN`, `RECCOM`, `SNDCOM`, `STACOM`, `TRMCOM`.  These are meant to implement light pen input and serial port I/O.
  * Some routines need testing with the appropriate hardware: `POLLEV`, `RDSTIK`, `RDTRIG`, `SNDLPT`.  These currently implement general event polling, joystick input, and printer output.

(Many of the needed OEM routines, such as `SETC` and `MAPXYC`, turn out to be
present in `BASICA.COM` from [Microsoft's earlier MS-DOS v1.25 code
release](https://github.com/microsoft/MS-DOS).  However, `BASICA.COM` is
only released in binary form, so some analysis is needed to extract the
routines.)

## Building instructions

### With JWasm and JWlink

* You need a system with AWK and GNU Make.  (AWK is used to run the script `jwasmify.awk` to munge the original sources into a form JWasm accepts.)
* Build or download binaries for [JWasm](https://github.com/Baron-von-Riedesel/JWasm) and [JWlink](https://github.com/JWasm/JWlink).  Install them.
* Run `make -f Makefile.jw`.

### With MASM 5.10A

Using  [DOSBox](https://www.dosbox.com/) mount a directory containing:
* This code
* The Microsoft Macro Assembler (MASM) version 5.1A (`masm.exe`).
* The Microsoft MAKE and LINK programs that come with MASM (`make.exe`, `link.exe`).

Run `make makefile` to assemble the files.
Note the tools may leave behind  party-built executables or object files.
If you want to rebuild them without changing the source code, you need
to delete these files by hand.

You can fetch MASM 5.1A from
[this site](https://www.pcjs.org/software/pcx86/lang/microsoft/masm/5.10x/) as follows.
* From the pull-down menu select `MS MAcro Assembler 5.10A (Update)`
* Press the `Load` button to load the disk image into the emulator
* Press the `Save` button to save the disk image to your computer
* Copy  the saved disk image to a Linux computer
* Mount the image using the command `sudo mount MASM51A-UPDATE.img /mnt`
* Copy the files from `/mnt` to your development directory

## License

All files within this repo are released under the [MIT (OSI) License]( https://en.wikipedia.org/wiki/MIT_License) as per the [LICENSE file](https://github.com/Microsoft/GW-BASIC/blob/master/LICENSE) stored in the root of this repo.

## Contributing

Pull requests addressing problems in getting GW-BASIC to build and run
are welcomed.

## Code of Conduct

https://github.com/fatman2021/GW-BASIC/blob/master/CODE_OF_CONDUCT.md