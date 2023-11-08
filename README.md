# OpenFOAM-AppleM1

Patch to compile OpenFOAM-v2306 on M1 Mac.

## OpenFOAM-v2306
### Procedures

1. Install these dependencies from homebrew
```
brew install cmake open-mpi libomp adios2 boost cgal fftw kahip metis petsc hypre
```

2. Install legacy `Scotch` (Thanks to @gerlero for creating this [tap](https://github.com/gerlero/homebrew-openfoam/tree/main/Formula))
```
brew tap gerlero/openfoam
brew install scotch-no-pthread
```

3. Create a **case-sensitive APFS volume** like this

![](https://develop.openfoam.com/Development/openfoam/-/wikis/images/apple-APFS-screenshot.png)

I usually create a soft link to keep everything consistent with Linux.
```
ln -s /Volumes/OpenFOAM ~/OpenFOAM
```

4. Clone the OpenFOAM source code into this volume
```
cd ~/OpenFOAM
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

6. Add OpenFOAM to `.zshrc` or `.bashrc`(if you wish)
```
echo 'source ~/OpenFOAM/OpenFOAM-v2306/etc/bashrc' >> ~/.zshrc
```
**Important:** you need to add the following for M1 (but not for x86):
```
export CPATH=/opt/homebrew/include
export LIBRARY_PATH=/opt/homebrew/lib
```

7. Compile the 
```
./Allwmake -j -s -l
```
It takes 40~45 minutes on M1.

You may add `-with-bear` option after installing `bear`.

See https://openfoamwiki.net/index.php/HowTo_Use_OpenFOAM_with_Visual_Studio_Code for more information

8. (Optional)Install `paraview` from Homebrew
```
brew install --cask paraview
```

### Known issue
Currently on Apple Silicon, floating point exception will trap SIGILL instead of SIGFPE. It is known that Asahi Linux can correctly trap SIGFPE so the problem is from macOS or XCode. The workaround is to use SIGILL for now. If you don't like this workaround, please submit a bug report to Apple (but don't expect they'll fix it).

It is suggested to run parallel command in scripts by using
```
. $WM_PROJECT_DIR/bin/tools/RunFunctions
runParallel ***Foam
```
to avoid macOS restrictions.

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
export WM_NCOMPPROCS=$(sysctl -n hw.ncpu)
./AllwmakeAll
```
It takes approximately 6 minutes on M1.

### Known issue
python 2.7 is removed in macOS 12.3+. Therefore `funkyPythonPostproc` is disabled.

## Benchmark
The [benchmark](https://github.com/BrushXue/OpenFOAM-AppleM1/raw/main/bench_template.zip) is modified from https://www.cfd-online.com/Forums/hardware/198378-openfoam-benchmarks-various-hardware.html
