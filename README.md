# OpenFOAM-AppleM1

Patch to compile OpenFOAM-v2106 on M1 Mac.

## OpenFOAM-v2106
### Procedures
1. Download and extract [OpenFOAM v2106 source code](https://dl.openfoam.com/source/v2106/OpenFOAM-v2106.tgz).
2. Apply macOS [patch](https://github.com/mrklein/openfoam-os-x/blob/master/OpenFOAM-v2106.patch) from mrklein.
3. Apply my patch for M1.
4. Modify third party library path as you wish (For ARM the path has moved to `/opt/homebrew/opt`).
And you probably need to add the following
```
export CPATH=/opt/homebrew/include
export LIBRARY_PATH=/opt/homebrew/lib
```
6. Compile the code. It takes approximately 35 minutes on M1.

### Known issues
sigFpe is disabled for now until new solution comes.

## swak4Foam
### Procedures
1. Apply my patch for macOS.
2. Add bison to the path:
```
export PATH="/opt/homebrew/opt/bison/bin:$PATH"
```
3. Compile the code. It takes approximately 6 minutes on M1.

### Known issues
System integrated python 2.7 cannot be linked.
```
ld: cannot link directly with dylib/framework, your binary is not an allowed client of /Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/System/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/config/libpython2.7.tbd for architecture arm64
```
