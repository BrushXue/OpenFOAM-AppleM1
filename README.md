# OpenFOAM-AppleM1

Patch to compile OpenFOAM-v2106 on M1 Mac.

## Procedures
1. Download and extract [OpenFOAM v2016 source code](https://dl.openfoam.com/source/v2106/OpenFOAM-v2106.tgz).
2. Apply macOS [patch](https://github.com/mrklein/openfoam-os-x/blob/master/OpenFOAM-v2106.patch) from mrklein.
3. Apply my patch for M1.
4. Modify third party path (/opt/homebrew/opt).
5. Compile the code. It takes approximately 35 minutes on M1.

## Known issues
sigFpe is disabled for now until new solution comes.
