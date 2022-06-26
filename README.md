# EDK2 files for VS2022

Modified files from EDK II for easier building with VS2022.  
Dump these files into EDK II's root directory and run:

    ./edksetup.bat Rebuild

and you can play with VS2022 happily. ~~Just a lazy tool for me~~

## What is changed

- Automatically generates a `VS2022` option for edksetup batch
- Target architecture is now default at `X64`
- Toolchain is now default at 64-bit VS2022 tools

## Defect?

When building against x64 targets, the /O1 optimize option in the compiler flag must be disabled at all time, debug or release.
The reason is that Visual C++ will try to turn some manual copying code into _memcpy_, which will not be resolved since they are non-EDK, external symbol.

I have noticed this happening with `MdeModulePkg/Universal/HiiDatabase`. Maybe there will be more.

This only happens with x64 targets. IA32 targets are immune to such issue.

I have found other people's complains about this issue at [here](https://www.mail-archive.com/edk2-devel@lists.sourceforge.net/msg09706.html)
and [here](https://www.mail-archive.com/edk2-devel@lists.01.org/msg11927.html). Been there for very long...

## Other notes

Some of the EDK II Win32 base tools from `Rebuild` will be built as 32-bit,
but this will not affect you building 64-bit targets at all.

Edk2 uses 32-bit version of Visual C++ tools.
If you want to use 64-bit tools, change the _vcvars.bat_ lines `243 - 263` in `BaseTools/set_vsprefix_envs.bat` to _vcvars64_.

But keep in mind that you have to do so **after building BaseTools**. Some conversion between 64-bit and 32-bit addresses
will prevent BaseTools from building.

## Licensing

This project inherits EDK II's [BSD + Patent License](https://github.com/tianocore/edk2/blob/master/License.txt).
