# OpenFOAM-AppleM1

Patch to compile OpenFOAM-v2312 on Apple Silicon.

## OpenFOAM-v2306
### Procedures

1. Install these dependencies from homebrew
```
brew install cmake open-mpi libomp adios2 boost cgal fftw kahip metis petsc scotch hypre
```

2. Create a **case-sensitive APFS volume** like this

![](https://develop.openfoam.com/Development/openfoam/-/wikis/images/apple-APFS-screenshot.png)

I usually create a soft link to keep everything consistent with Linux.
```
ln -s /Volumes/OpenFOAM ~/OpenFOAM
```

3. Clone the OpenFOAM source code into this volume
```
cd ~/OpenFOAM
git clone https://develop.openfoam.com/Development/openfoam.git OpenFOAM-v2312
cd OpenFOAM-v2312
git checkout OpenFOAM-v2312
```
**Optional:** adding submodules such as petsc4Foam
```
git submodule init
git submodule update
```

4. Apply my patch for M1.
```
curl -OL https://github.com/BrushXue/OpenFOAM-AppleM1/raw/main/M1.patch
git apply M1.patch
```

5. Add OpenFOAM to `.zshrc` or `.bashrc`(if you wish)
```
echo 'source ~/OpenFOAM/OpenFOAM-v2312/etc/bashrc' >> ~/.zshrc
```
**Important:** you need to add the following for M1 (but not for x86):
```
export CPATH=/opt/homebrew/include
export LIBRARY_PATH=/opt/homebrew/lib
```

6. Compile the code
```
./Allwmake -j -s -l
```
It takes 40~45 minutes on M1.

**Optional:** you may add `-with-bear` option after installing `bear`.

See https://openfoamwiki.net/index.php/HowTo_Use_OpenFOAM_with_Visual_Studio_Code for more information

**Optional:** Install `paraview` from Homebrew
```
brew install --cask paraview
```

### Known issue
Currently on Apple Silicon, floating point exception will trap SIGILL instead of SIGFPE. It is known that Asahi Linux can correctly trap SIGFPE so the problem is from macOS or XCode. The workaround is to use SIGILL for now. If you don't like this workaround, please submit a bug report to Apple (but don't expect they'll fix it).
