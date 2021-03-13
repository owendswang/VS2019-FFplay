# VS2019 FFplay
FFplay built with Visual Studio 2019

## Introduction
This is a VS2019 C++ project of ffplay. But according to the complicated issues with licenses and copyrights, I didn't include any FFmpeg and SDL2 source files or lib files. But you could add them by yourself according to the instructions below. So this repository only contains VS project and solution files.

## Versions
- Source Files:            FFmpeg-n4.3.2
- FFmpeg Windows build:    ffmpeg-4.3.2-2021-02-27-full_build-shared
- SDL2 Windows build:      SDL2-devel-2.0.14-VC
- Visual Studio:           2019

## Platform
Windows 10 x64 (no x86 support)

## Instructions
1. Configure FFmpeg source files:
   1. Download FFmpeg source files from [https://github.com/FFmpeg/FFmpeg/releases](https://github.com/FFmpeg/FFmpeg/releases)
   2. Download MSYS2 from [https://www.msys2.org/](https://www.msys2.org/)
   3. Install MSYS2.
   4. Install necessary packages:
      Run "MSYS2 MinGW 64-bit" from Start menu and input the follwing lines:
      ```
      pacman -Syu
      pacman -S make pkg-config diffutils yasm
      pacman -S mingw-w64-x86_64-nasm mingw-w64-x86_64-gcc mingw-w64-x86_64-SDL2
      ```
   5. Make MSYS2 use VS build tools:
      Create a batch file called "msys_vs2019.bat", and fill in the following lines:
      ```
      set MSYS2_PATH_TYPE=inherit
      call "C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\VC\Auxiliary\Build\vcvars64.bat"
      msys2_shell.cmd -mingw64
      ```
      Rename "C:\msys2\usr\bin\link.exe" to "C:\msys2\usr\bin\link.exe.bak".
   6. Configure FFplay:
      Unpack FFmpeg source files anywhere you like. And in "MSYS2 MinGW 64-bit" shell, "cd" into the source files folder. Input the fowllowing lines:
      ```
      ./configure --enable-shared --prefix=. --toolchain=msvc --arch=x86_64 --target-os=win64
      ```
2. Create the "FFplay" C++ project:
   1. Open Visual Studio 2019 and create an empty C++ project called "FFplay".
   2. Download FFmpeg shared buid from [https://www.gyan.dev/ffmpeg/builds/](https://www.gyan.dev/ffmpeg/builds/).
      Make sure it's the same version with the source files you used above.
   3. From the FFmpeg build ".zip" file, unpack "include" and "lib" folder into our project folder.
      And unpack all the "dll" files in the "bin" folder to "x64/Debug" folder in our project folder.
   4. Download SDL2 developer build from [https://www.libsdl.org/download-2.0.php](https://www.libsdl.org/download-2.0.php).
      Make sure it's windows vc developer version.
   5. From the SDL2 build ".zip" file, unpack all the files in "include" folder to "include/SDL2" folder in our project folder.
      And unpack all the "lib" files from "lib/x64" folder to "lib" folder in our project folder.
      And unpack the "dll" file from "lib/x64" folder to "x64/Debug" folder in our project folder.
   6. Copy the "config.h" file from FFmpeg source file to our project folder. And copy the "ffplay.c", "cmdutils.h", "cmdutils.c" from "fftools" folder in the source files to our project foler.
3. Modify our "FFplay" project:
   1. Right click our project "FFplay" and click "Add" -> "Existing item...". And select all 4 files we copied ("config.h", "ffplay.c", "cmdutils.h", "cmdutils.c").
   2. Set project property:
      - "Configuration": `Debug`; "Platform": `x64`
      - "C/C++" -> "General" -> "Additional Include Directories":
        ```
        $(ProjectDir)include;%(AdditionalIncludeDirectories)
        ```
      - "C/C++" -> "General" -> "SDL checks": `No (/sdl-)`
      - "Linker" -> "General" -> "Additional Library Directories":
        ```
        $(ProjectDir)lib;%(AdditionalLibraryDirectories)
        ```
      - "Linker" -> "General" -> "Additional Dependencies":
        ```
        avcodec.lib;avdevice.lib;avfilter.lib;avformat.lib;avutil.lib;postproc.lib;SDL2.lib;SDL2main.lib;SDL2test.lib;swresample.lib;swscale.lib;%(AdditionalDependencies)
        ```
   3. Tweak source code:
      - "cmdutils.c": Comment out all the lines related with "avresample". This function is deprecated and not enabled in the FFmpeg lib we use. Basically everything marked as error.
      - "ffplay.c": Replace `#include <SDL.h>` with `#include "SDL2/SDL.h"`. And replace `#include <SDL_thread.h>` with `#include "SDL2/SDL_thread.h"`.
4. Debug our "FFplay" project:
   - Now there should be no error in our project.
   - To make it play a video when we click "Local Windows Debugger", we need to add an argument to our debug command.
     Project Property: "Configuration Properties" -> "Debugging" -> "Command Arguments": `$(Your_Video_File_Path)`

## Reference
- https://www.pianshen.com/article/38081048616/
- https://blog.csdn.net/Tui_GuiGe/article/details/90320224
- https://zhuanlan.zhihu.com/p/205156264

## License
Everything I created would follow MIT. But everything related with FFmpeg and SDL2, please follow their licenses and copyrights.
