# OpenFOAM-AppleM1

Patch to compile OpenFOAM-v2306 on M1 Mac.

## OpenFOAM-v2306
### Procedures
1. Create to a **case-sensitive volume** like this

![](https://develop.openfoam.com/Development/openfoam/-/wikis/images/apple-APFS-screenshot.png)

I usually create a soft link so I don't need to change `etc/bashrc`
```
ln -s /Volumes/OpenFOAM ~/OpenFOAM
cd ~/OpenFOAM
```

2. Install these components from homebrew
```
brew install cmake open-mpi libomp adios2 boost cgal fftw kahip metis petsc hypre
```

3. Install modifiled `scotch` (Thanks to @gerlero for creating this [tap](https://github.com/gerlero/homebrew-openfoam/tree/main/Formula))
```
brew tap gerlero/openfoam
brew install scotch-no-pthread
```

4. Clone the OpenFOAM source code into this volume
```
git clone https://develop.openfoam.com/Development/openfoam.git OpenFOAM-v2306
cd OpenFOAM-v2306
git checkout OpenFOAM-v2306
git submodule init
git submodule update
```

5. Apply my patch for M1.
```
curl -OL https://github.com/BrushXue/OpenFOAM-AppleM1/raw/main/M1.patch
git apply M1.patch
```

6. Add OpenFOAM to `.zshrc` or `.bashrc`
```
echo 'source ~/OpenFOAM/OpenFOAM-v2306/etc/bashrc' >> ~./zshrc
```
And you probably need to add the following for M1:
```
export CPATH=/opt/homebrew/include
export LIBRARY_PATH=/opt/homebrew/lib
```

7. Compile the code with [bear](https://openfoamwiki.net/index.php/HowTo_Use_OpenFOAM_with_Visual_Studio_Code) (remove `-with-bear` if you don't need it).
```
./Allwmake -j -s -l -with-bear
```
It takes 40~45 minutes on M1.

8. (Optional)Install `paraview` from Homebrew
```
brew install --cask paraview
```

### Known issue
sigFpe is disabled for now until new solution comes.

It is suggested to run parallel command in scripts by using
```
. $WM_PROJECT_DIR/bin/tools/RunFunctions
runParallel ***Foam
```
to avoid macOS restrictions.

## swak4Foam
### Procedures
1. Install these components from homebrew:
```
brew install bison pkgconfig mercurial
```
2. Download swak4Foam
```
hg clone http://hg.code.sf.net/p/openfoam-extend/swak4Foam swak4Foam
cd swak4Foam
hg update develop
```
3. Apply my patch for macOS.
```
curl -OL https://github.com/BrushXue/OpenFOAM-AppleM1/raw/main/swak4Foam.patch
git apply swak4Foam.patch
```
4. Add bison to the path:
```
export PATH="$(brew --prefix)/opt/bison/bin:$PATH"
```
5. Compile the code.
```
export WM_NCOMPPROCS=$(sysctl -n hw.ncpu)
./AllwmakeAll
```
It takes approximately 6 minutes on M1.

### Known issue
python 2.7 is removed in macOS 12.3+. Therefore `swakPythonIntegration` and `funkyPythonPostproc` are disabled.

## Benchmark
The [benchmark](https://github.com/BrushXue/OpenFOAM-AppleM1/raw/main/bench_template.zip) is modified from https://www.cfd-online.com/Forums/hardware/198378-openfoam-benchmarks-various-hardware.html
