# Standalone xulrunner-stub project

This project continues to maintain xulrunner-stub sources extracted from mozilla's repository and makes it built against Firefox SDK. Background: mozilla had removed xulrunner sources from their repository ([issue](https://bugzilla.mozilla.org/show_bug.cgi?id=1221724)).

## Pre build binaries
You can download them from the [project page](https://duanyao.github.io/xulrunner-stub/).

## Build for Win32

0. Install MS Visual Studio 2013 (Express for Desktop or Community Editions are OK).
1. Clone this project.
2. Download Firefox SDK. e.g. 
http://ftp.mozilla.org/pub/firefox/nightly/latest-mozilla-aurora/firefox-47.0a2.en-US.win32.sdk.zip or http://archive.mozilla.org/pub/firefox/releases/45.0/win32/en-US/sdk/firefox-45.0.sdk.zip .
Unzip it in the project directory, and make sure the directory name is `firefox-sdk`.

3. Open `msvc/xulrunner-stub.sln` with Visual Studio, and build in release mode (debug mode doesn't work currently).
If all is OK, you'll get `msvc/Release/xulrunner-stub.exe`.

## Run your xulrunner app & runtime dependencies
You may layout your xulrunner app like this:

```
<your_app_dir>
  xulrunner/
  mozglue.dll
  msvcr120.dll
  msvcp120.dll
  xulrunner-stub.exe
  application.ini
  ...
```
Notes
* You may copy firefox's files (`xul.dll`, `omni.ja` etc.) to `xulrunner/` to act as xulrunner.
* I'm not able to remove the stub's hard dependency on `mozglue.dll`, `msvcr120.dll` and `msvcp120.dll`, so you have to copy them from firefox to the same directory of the stub.

Alternatively, you may "flatten" `xulrunner/` into `<your_app_dir>`:

```
<your_app_dir>
  xul.dll
  omni.ja
  ...
  mozglue.dll
  msvcr120.dll
  msvcp120.dll
  xulrunner-stub.exe
  application.ini
  ...
```

In this way, you don't have to duplicate `mozglue.dll`, `msvcr120.dll` and `msvcp120.dll`.
"Flatten xulrunner" is a new feature introduced by commit `5bb9c7f4addab9b745b9186da4dcade8720378c8` .

## Build for other platforms
Patches are welcome!

## Source files' origin
* `xulrunner/stub/nsXULStub.cpp`: The main source file, was removed from mozilla's repository.
* `xpcom/build/nsXPCOMPrivate.h`: Missing from firefox sdk, so copied to this project.
* `toolkit/xre/nsWindowsWMain.cpp`: Ditto.

## Portable profile

This a new feature which makes it possible to store user's profile inside `<your_app_dir>`: If a directory named `PortableProfile` exits in `<your_app_dir>`, it will be used as the profile directory ([implementation details](https://github.com/duanyao/xulrunner-stub/commit/9e375e5157fed456ba8c285ad7e002cd9dac7d6b)). Note that command line option `-profile` can still override this behaviour. This feature currently lives in branch `PortableProfile`, and there are binaries with this feature in the [project page](https://duanyao.github.io/xulrunner-stub/).
