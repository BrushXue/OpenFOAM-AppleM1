# OpenFOAM-AppleM1

Patch to compile OpenFOAM-v2112 on M1 Mac.

## OpenFOAM-v2112
### Procedures
1. Download and extract [OpenFOAM v2112 source code](https://dl.openfoam.com/source/v2112/OpenFOAM-v2112.tgz) in a case-sensitive volume.
2. Apply my patch for M1.
```
git apply M1.patch
```
3. Install these components from homebrew
```
brew install cmake open-mpi libomp adios2 boost fftw kahip metis 
```
4. Install modifiled `scotch` and `CGAL@4` (Thanks to @gerlero for creating this [tap](https://github.com/gerlero/homebrew-openfoam/tree/main/Formula))
```
brew tap gerlero/openfoam
brew install scotch-no-pthread cgal@4
```
And you probably need to add the following:
```
export CPATH=/opt/homebrew/include
export LIBRARY_PATH=/opt/homebrew/lib
```
5. Compile the code with [bear](https://openfoamwiki.net/index.php/HowTo_Use_OpenFOAM_with_Visual_Studio_Code) (remove `-with-bear` if you don't need it).
```
./Allwmake -j -s -l -with-bear
```
It takes approximately 35 minutes on M1.

6. Install `paraview` from Homebrew
```
brew install --cask paraview
```

### Known issue
sigFpe is disabled for now until new solution comes.

## swak4Foam
### Procedures
1. Download swak4Foam
```
hg clone http://hg.code.sf.net/p/openfoam-extend/swak4Foam swak4Foam
cd swak4Foam
hg update develop
```
2. Apply my patch for macOS.
```
git apply swak4Foam.patch
```
4. Add bison to the path:
```
export PATH="/opt/homebrew/opt/bison/bin:$PATH"
```
4. Compile the code.
```
./AllwmakeAll
```
It takes approximately 6 minutes on M1.

### Known issue
python 2.7 is removed in macOS 12.3+. Therefore `swakPythonIntegration` and `funkyPythonPostproc` are disabled.
