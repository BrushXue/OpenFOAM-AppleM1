# OpenFOAM-AppleM1

Patch to compile OpenFOAM-v2206 on M1 Mac.

## OpenFOAM-v2206
### Procedures
1. Install these components from homebrew
```
brew install cmake open-mpi libomp adios2 boost fftw kahip metis 
```
2. Install modifiled `scotch` and `CGAL@4` (Thanks to @gerlero for creating this [tap](https://github.com/gerlero/homebrew-openfoam/tree/main/Formula))
```
brew tap gerlero/openfoam
brew install scotch-no-pthread cgal@4
```
3. Create to a **case-sensitive volume** like this

![](https://develop.openfoam.com/Development/openfoam/-/wikis/images/apple-APFS-screenshot.png)

I usually create a soft link so I don't need to change `etc/bashrc`
```
ln -s /Volumes/OpenFOAM ~/OpenFOAM
cd ~/OpenFOAM
```
then clone the OpenFOAM source code into this volume
```
git clone https://develop.openfoam.com/Development/openfoam.git OpenFOAM-v2206
cd OpenFOAM-v2206
git checkout OpenFOAM-v2206
git submodule init
git submodule update
```
4. Apply my patch for M1. x86 should be compatible as well.
```
curl -OL https://github.com/BrushXue/OpenFOAM-AppleM1/raw/main/M1.patch
git apply M1.patch
```
5. Fix `DYLD_LIBRARY_PATH` issue (Thanks Apple for implementing this [stupid feature](https://briandfoy.github.io/macos-s-system-integrity-protection-sanitizes-your-environment/).)
```
echo 'export FOAM_DYLD_LIBRARY_PATH="$DYLD_LIBRARY_PATH"' >> etc/bashrc
```
and you may need to add the following to your bash/zsh script running OpenFOAM executables.
```
source $WM_PROJECT_DIR/etc/bashrc
```
6. And you probably need to add the following for M1:
```
export CPATH=/opt/homebrew/include
export LIBRARY_PATH=/opt/homebrew/lib
```
7. Add OpenFOAM to `.zshrc` or `.bashrc`
```
echo 'source ~/OpenFOAM/OpenFOAM-v2206/etc/bashrc' >> ~./zshrc
```
8. Compile the code with [bear](https://openfoamwiki.net/index.php/HowTo_Use_OpenFOAM_with_Visual_Studio_Code) (remove `-with-bear` if you don't need it).
```
./Allwmake -j -s -l -with-bear
```
It takes 40~45 minutes on M1.

9. Install `paraview` from Homebrew
```
brew install --cask paraview
```

## Known Issue
You may see these error messages on x86 Macs. It's not happening on M1. Looks like there's a bug in Apple Clang 14.
```
clang: error: unable to execute command: Segmentation fault: 11
clang: error: clang frontend command failed due to signal (use -v to see invocation)
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
curl -OL https://github.com/BrushXue/OpenFOAM-AppleM1/raw/main/swak4Foam.patch
git apply swak4Foam.patch
```
3. Install these components from homebrew:
```
brew install bison pkgconfig
```
4. Add bison to the path:
```
export PATH="$(brew --prefix)/opt/bison/bin:$PATH"
```
5. Compile the code.
```
./AllwmakeAll
```
It takes approximately 6 minutes on M1.

### Known issue
python 2.7 is removed in macOS 12.3+. Therefore `swakPythonIntegration` and `funkyPythonPostproc` are disabled.

## Benchmark
The [benchmark](https://github.com/BrushXue/OpenFOAM-AppleM1/raw/main/bench_template.tgz) is modified from https://www.cfd-online.com/Forums/hardware/198378-openfoam-benchmarks-various-hardware.html
