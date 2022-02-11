# OpenFOAM-AppleM1

Patch to compile OpenFOAM-v2112 on M1 Mac.

## OpenFOAM-v2112
### Procedures
1. Download and extract [OpenFOAM v2112 source code](https://dl.openfoam.com/source/v2112/OpenFOAM-v2112.tgz).
2. Apply my patch for M1.
3. Remove scotch from Homebrew(if exists) and install mine formula.

And you probably need to add the following
```
export CPATH=/opt/homebrew/include
export LIBRARY_PATH=/opt/homebrew/lib
```
4. Compile the code. It takes approximately 35 minutes on M1.

### Known issue
sigFpe is disabled for now until new solution comes.

## swak4Foam
### Procedures
1. Apply my patch for macOS.
2. Add bison to the path:
```
export PATH="/opt/homebrew/opt/bison/bin:$PATH"
```
3. Compile the code. It takes approximately 6 minutes on M1.

### Known issue
System integrated python 2.7 cannot be linked.
```
ld: cannot link directly with dylib/framework, your binary is not an allowed client of /Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/System/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/config/libpython2.7.tbd for architecture arm64
```
