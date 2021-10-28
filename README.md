# OpenFOAM-AppleM1

Patch to compile OpenFOAM-v2106 on M1 Mac.

## Procedures
1. Download and extract [OpenFOAM v2016 source code](https://dl.openfoam.com/source/v2106/OpenFOAM-v2106.tgz).
2. Apply macOS [patch](https://github.com/mrklein/openfoam-os-x/blob/master/OpenFOAM-v2106.patch) from mrklein.
3. Apply my patch for M1

## Known issues
1. Missing header in homebrew [kahip](https://github.com/Homebrew/homebrew-core/blob/HEAD/Formula/kahip.rb).
2. Missing header in homwbrew [ptscotch](https://github.com/Homebrew/homebrew-core/blob/HEAD/Formula/scotch.rb).
3. sigFpe is disabled for now until new solution comes.
