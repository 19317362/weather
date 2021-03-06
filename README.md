Simple cross-platform open source weather by city application. Hunter package manager usage example.

### Used service

* http://openweathermap.org/api

### C/C++ core

* [Boost](http://www.boost.org/)
* [Sober](http://github.com/ruslo/sober)
* [CppNetlib.URI](https://github.com/cpp-netlib/uri)
* [GTest](http://code.google.com/p/googletest/)
* [JSONSpirit](https://github.com/cierelabs/json_spirit)

### Build system

* [CMake](http://cmake.org/)
 * with modules:
  * [Sugar](https://github.com/ruslo/sugar) - tools, wrappers, workarounds, ...
  * [Polly](https://github.com/ruslo/polly) - toolchain files
  * [Hunter](https://github.com/ruslo/hunter) - package manager

### Requirements

* CMake version 3.0 ([more](https://github.com/ruslo/hunter/wiki/Requirements#cmake-30))
* `HUNTER_ROOT` environment variable ([more](https://github.com/ruslo/hunter/wiki/Requirements#hunter_root))
* Python 3 (For `Xcode` and `Visual Studio` based projects)
* `POLLY_ROOT` environment variable (for toolchain-based builds, [more](https://github.com/ruslo/hunter/wiki/Requirements#toolchains-example-polly))

### Usage

*Note* that a lot of time (> 1 hour) and space (> 1 GB) may be required for build, so be patient and
consider test this [tiny-project](https://github.com/forexample/hunter-simple) before run.

#### Download and unpack

Download and unpack current release archive and cd to unpacked directory.
Unix-style:

```
> wget https://github.com/ruslo/weather/archive/v1.2.0.tar.gz
> tar xf v1.2.0.tar.gz
> cd weather-1.2.0
```

#### Build and test

* [Windows Visual Studio](https://github.com/ruslo/weather#windows-visual-studio-tested-with-2013-3264)
* [Windows Cygwin](https://github.com/ruslo/weather#windows-cygwin)
* [Linux](https://github.com/ruslo/weather#linux)
* [Mac Xcode](https://github.com/ruslo/weather#mac-xcode)
* [Mac Makefile](https://github.com/ruslo/weather#mac-makefile)
* [Mac iOS](https://github.com/ruslo/weather#mac-ios)

##### Windows (Visual Studio, tested with 2013 32/64)

* Run cmd and check cmake version, `HUNTER_ROOT` environment, python 3:
```
> where cmake
\path\to\cmake
> cmake --version
cmake version 3.0.0
> where python
\path\to\python.exe
> python --version
Python 3.x.x
> echo %HUNTER_ROOT%
\path\to\hunter\root
```
* Start build:
```
> cmake -H. -B_builds -DHUNTER_STATUS_DEBUG=ON -G"Visual Studio 12 2013 Win64"
> cmake --build _builds --config Release
```

* Test it:
```
> _builds\Release\weather-cli.exe Madrid > result.txt
# open result.txt with utf-8 friendly text editor (notepad fits fine)
```

##### Windows (cygwin)

* Not working (`g++-4.8.3` don't have `std::to_string` which is used by CppNetlib.URI)

##### Linux

* Check cmake version, `HUNTER_ROOT` and `POLLY_ROOT` environment variables:
```
> which cmake
/path/to/cmake
> cmake --version
cmake version 3.0.0
> echo $HUNTER_ROOT
/path/to/hunter/root/
> echo $POLLY_ROOT
/path/to/toolchains
```

* Pick toolchain with `c++11` support, for example [gcc](https://github.com/ruslo/polly/wiki/Toolchain-list#gcc)
(`gcc -std=c++11`):
```
> ls $POLLY_ROOT/gcc.cmake
/path/to/toolchains/gcc.cmake
```

* Start build:
```
> cmake -H. -B_builds -DHUNTER_STATUS_DEBUG=ON -DCMAKE_TOOLCHAIN_FILE=$POLLY_ROOT/gcc.cmake
> cmake --build _builds
```

* Test it:
```
> ./_builds/weather-cli Madrid
City: Madrid
Success...
  longitude: -3.7
  latitude: 40.42
  temperature: 17.17
  temperature(human): +17.2 ℃
  description: sky is clear
  icon: 02d
Done
```

##### Mac (Xcode)

* Check cmake version, python 3, `HUNTER_ROOT` and `POLLY_ROOT` environment variables:
```
> which cmake
/path/to/cmake
> cmake --version
cmake version 3.0.0
> which python3
/path/to/python3
> echo $HUNTER_ROOT
/path/to/hunter/root/
> echo $POLLY_ROOT
/path/to/toolchains
```

* Check [xcode](https://github.com/ruslo/polly/wiki/Toolchain-list#xcode) toolchain and start build:
```
> ls $POLLY_ROOT/xcode.cmake
/path/to/toolchains/xcode.cmake
> cmake -H. -B_builds -DHUNTER_STATUS_DEBUG=ON -DCMAKE_TOOLCHAIN_FILE=$POLLY_ROOT/xcode.cmake -GXcode
> cmake --build _builds --config Release
```

* Test it:
```
> ./_builds/Release/weather-cli Madrid
City: Madrid
Success...
  longitude: -3.7
  latitude: 40.42
  temperature: 21.69
  temperature(human): +21.7 ℃
  description: Sky is Clear
  icon: 01d
Done
```

##### Mac (Makefile)

* Check cmake version, `HUNTER_ROOT` and `POLLY_ROOT` environment variables:
```
> which cmake
/path/to/cmake
> cmake --version
cmake version 3.0.0
> echo $HUNTER_ROOT
/path/to/hunter/root/
> echo $POLLY_ROOT
/path/to/toolchains
```

* Pick toolchain with `c++11` support, for example [libcxx](https://github.com/ruslo/polly/wiki/Toolchain-list#libcxx)
(`clang -std=c++11 -stdlib=libc++`):
```
> ls $POLLY_ROOT/libcxx.cmake
/path/to/toolchains/libcxx.cmake
```

* Start build:
```
> cmake -H. -B_builds -DHUNTER_STATUS_DEBUG=ON -DCMAKE_TOOLCHAIN_FILE=$POLLY_ROOT/libcxx.cmake
> cmake --build _builds
```

* Test it:
```
> ./_builds/weather-cli Madrid
City: Madrid
Success...
  longitude: -3.7
  latitude: 40.42
  temperature: 17.71
  temperature(human): +17.7 ℃
  description: Sky is Clear
  icon: 01d
Done
```

##### Mac (iOS)

* Build patched cmake and add it to the `PATH`:
```
> CMAKE_VERSION="3.0.0-ios-universal"
> wget "https://github.com/ruslo/CMake/archive/v${CMAKE_VERSION}.tar.gz"
> tar xf "v${CMAKE_VERSION}.tar.gz"
> cd "CMake-${CMAKE_VERSION}"
> cmake -H. -B_builds -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX="`pwd`/_install" -DCMAKE_USE_SYSTEM_CURL=YES
> cmake --build _builds --target install
> export PATH="`pwd`/_install/bin:${PATH}"
> cd ..
```

* Check cmake version, python 3, `HUNTER_ROOT` and `POLLY_ROOT` environment variables:
```
> which cmake
/path/to/patched/cmake/version/_install/bin/cmake
> cmake --version
cmake version 3.0.0
> which python3
/path/to/python3
> echo $HUNTER_ROOT
/path/to/hunter/root/
> echo $POLLY_ROOT
/path/to/toolchains
```

* Check [ios](https://github.com/ruslo/polly/wiki/Toolchain-list#ios) toolchain and install dependencies:
```
> ls $POLLY_ROOT/ios.cmake
/path/to/toolchains/ios.cmake
> cmake -H. -B_builds -DHUNTER_STATUS_DEBUG=ON -DCMAKE_TOOLCHAIN_FILE=$POLLY_ROOT/ios.cmake -GXcode
```

* Open project in `Xcode` and build/run on target you need:
```
> open _builds/Weather.xcodeproj
```

[![AppStore][appstore_logo]][weather_link]

[appstore_logo]: https://linkmaker.itunes.apple.com/htmlResources/assets/en_us//images/web/linkmaker/badge_appstore-lrg.svg
[weather_link]: https://itunes.apple.com/us/app/weather-with-hunter/id885350236?mt=8&uo=4
