# CMAKE Tutorial

Following Tutorial to CMake 3.18 (hopefully get to modules)

## Requirement

- `clang 18`
- `cmake 3.28`

## How to use

- Create a sub directory like `mkdir build`
- Move to the subdirectory `cd build`
- Run cmake that is in the above dir `cmake ..`
- Run make to compile and run `make` or `cmake --build .`

### for ide

`cmake -S <SourceDir> -B <BuildDir>`
`cmake --build <BuildDir>` or `cd <BuidDir> && make`

### Parallel processing fl

Use `-j <N>`  flag or `--parallel <N>` use `CMAKE_BUILD_PARALLEL_LEVEL`

### Debugging the cmake process

Use `cmake ... --trace` or `cmake ... --log-level=Verbose` (can use Error, Status, )

### With different DEBUG / RELEASE

- `cmake --install .`
- `cmake --install . --config Release`
- `cmake --build . --target install --config Release`
- `cmake --install . --prefix "/home/myuser/installdir`
