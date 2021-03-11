# VS2019 FFplay
FFplay built with Visual Studio 2019

## Versions
- Source Files:            FFmpeg-n4.3.1
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
3. 
