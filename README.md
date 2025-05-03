# devcontainer-labo-11

## Devcontainer

Basic C and C++ devcontainer with cmake, compiler warnings, clang-tidy, and clang-format.

### Installed tools

* lldb
* clang
* cmake
* clang-tidy
* clang-format

### Supported IDEs

* vscode
* clion

## Adding exercises

* Create a new directory for each exercise in the `src` directory
* Add `CMakeLists.txt` in the new directory

```cmake
cpprog_add_library(
    TARGET exercise_1_lib   # library will be called exercise_1_lib
    CXX_MODULES             # module source files here
    "my_module_1.cpp"
    "my_module_2.cpp"
    DEPENDENCIES            # libraries on which the library depends
    datetime
)

cpprog_add_executable(
    TARGET exercise_1       # executable will be called exercise_1
    CXX_MODULES             # module source files here
    "my_module_1.cpp"
    "my_module_2.cpp"
    CXX_SOURCES             # old-style source files here
    "main.cpp"
    DEPENDENCIES            # libraries on which the exercise depends
    exercise_1_lib
)
```

* Add the new directory to the `src/CMakeLists.txt` file after the line `add_subdirectory(cpprog)`

```cmake
add_subdirectory(my_new_directory)
```

* Configure/build the project using the buttons in the vscode status bar
* Select the target to run in vscode

```text
View > Command Palette... > CMake: Set Launch/Debug Target
```

* Run/Debug the selected target executable using the buttons in the vscode status bar
* Add unittest source files to the `test` directory
* Add unittests to the `test/CMakeListst.txt` file

```cmake
cpprog_add_test(
    TARGET test_exercise_1  # test will be called test_exercise_1
    CXX_SOURCES             # unittest source files
    "my_module_1.test.cpp"
    DEPENDENCIES            # libraries on which the test depends
    exercise_1_lib
)
```

* Run test from the `Testing` activity in the vscode action bar
* View test results in the `Test Results` tab in the vscode bottom panel

## Using CLion instead of vscode

1. **Settings > Build, Execution, Deployment > Toolchains**
   * **CMake**: Change from **Bundled** to **Custom CMake executable** with value **/usr/local/bin/cmake**
   * **Debugger**: Change from **Bundled GDB** to **Custom LLDB executable** with value **/usr/bin/lldb**
2. **Settings > Build, Execution, Deployment > CMake**
   * Delete **Debug** preset
   * Enable **clang-debug** and **clang-release** presets
3. **Settings > Build, Execution, Deployment > Dynamic Analysis Tools > Sanitizers**
   * **LeakSanitizer**: Set **LSAN_OPTIONS** field to **detect_leaks=0** (disables leak detection, required for running with the debugger)
