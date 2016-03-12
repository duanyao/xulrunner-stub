# Standalone xulrunner-stub project

This project continues to maintain xulrunner-stub sources extracted from mozilla's repository and makes it built against Firefox SDK. Background: mozilla had removed xulrunner sources from their repository ([issue](https://bugzilla.mozilla.org/show_bug.cgi?id=1221724)).

## Build for Win32

0. Install MS Visual Studio 2013 (Express for Desktop or Community Editions are OK).
1. Clone this project.
2. Download Firefox SDK. e.g. 
http://ftp.mozilla.org/pub/firefox/nightly/latest-mozilla-aurora/firefox-47.0a2.en-US.win32.sdk.zip .
Unzip it in the project directory, and make sure the directory name is `firefox-sdk`.

3. Open `msvc/xulrunner-stub.sln` with Visual Studio, and build in release mode (debug mode doesn't work currently).
If all is OK, you'll get `msvc/Release/xulrunner-stub.exe`.

## Run your xulrunner app
Layout your xulrunner app like this:

```
<your_app_dir>
  xulrunner/            <- copy firefox's files here.
  xulrunner-stub.exe
  mozglue.dll           <- copy firefox's mozglue.dll here.
  application.ini
  ...
```

Notes
* You may use firefox acting as xulrunner.
* I'm not able to remove `xulrunner-stub.exe`'s dependency on `mozglue.dll` so you have to place the latter under the same directory of the former.

## Build for other platforms
Patches are welcome!

## Source files' origin
* `xulrunner/stub/nsXULStub.cpp`: The main source file, was removed from mozilla's repository.
* `xpcom/build/nsXPCOMPrivate.h`: Missing from firefox sdk, so copied to this project.
* `toolkit/xre/nsWindowsWMain.cpp`: Ditto.
