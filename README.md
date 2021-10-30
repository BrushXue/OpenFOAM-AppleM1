# OpenFOAM-AppleM1

Patch to compile OpenFOAM-v2106 on M1 Mac.

## OpenFOAM-v2106
### Procedures
1. Download and extract [OpenFOAM v2106 source code](https://dl.openfoam.com/source/v2106/OpenFOAM-v2106.tgz).
2. Apply macOS [patch](https://github.com/mrklein/openfoam-os-x/blob/master/OpenFOAM-v2106.patch) from mrklein.
3. Apply my patch for M1.
4. Modify third party library path as you wish (For ARM the path has moved to `/opt/homebrew/opt`).
5. Compile the code. It takes approximately 35 minutes on M1.

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
