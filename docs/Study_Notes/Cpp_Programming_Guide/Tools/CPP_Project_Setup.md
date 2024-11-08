---
id: CPP_Project_Setup
aliases:
  - Project Setup
tags: []
---

# Project Setup


## File Structure

The Most Basic structure of a c++ project looks as follows:

```bash
❯ tree -L 1
[ 332]  ./
├── [ 346]  build/
├── [   0]  ext/
├── [   0]  include/
├── [  16]  src/
├── [   0]  tests/
├── [ 518]  vcpkg/
├── [ 186]  CMakeLists.txt
├── [  82]  compile_commands.json -> /home/parth/Desktop/programming/cpp/learncpp.com/chap0/build/compile_commands.json
├── [   0]  README.md
├── [4.1K]  tasks.py
├── [ 348]  vcpkg-configuration.json
└── [  38]  vcpkg.json
```

### Organization

#### Systems

1. `CMake` : Used to generate Build System Configurations.
2. `Ninja` : Build system to compile the project.
3. `gcc` : Compiler for the project.
4. `vcpkg` : Package Manager for C/C++. Should be added as submodule to your project.
5. `invoke` : Python Library to run the CMake Configurations.

#### Folders

1. `build/` : Stores the build files and executables
2. `ext/` : Stores external libraries
3. `include/` : Stores the `.h` and `.hpp` header files made by us.
4. `src/` : Holds the `.c` and `.cpp` files made by us.
5. `tests/` : Holds the Tests for the codebase
6. `vcpkg/` : Holds Microsoft C++ Package Manager

#### Files

1. `CmakeLists.txt` : Contains the build guide for the codebase including pointing towards the needed libraries and compilation options.
2. `compile_commands.json` : Contains the Intellisense information for the IDEs.
3. `README.md` : Project Information for people to build and understand
4. `tasks.py` : Script containing `invoke` which makes it easier to call CMake with custom commands.
5. `vcpkg.json` : Used by `vcpkg` to hold the libraries requested by user.

---

## Setup Process

1. `mkdir && cd` Project Directory

2. Initialize Git
    ```bash
    git init
    ```
3. Add `vcpkg` as submodule to your project
    ```bash
    git submodule add https://github.com/microsoft/vcpkg.git
    git submodule update --init
    cd vcpkg
    ./bootstrap-vcpkg.sh -disableMetrics
    ```
4. Add the essential libraries for the project using `vcpkg`:
    1. Initialize `vcpkg`:
    ```bash
    vcpkg new --application
    ```
    2. Add Libraries:
    ```bash
    vcpkg add port <library_name>
    ```
5. Write the `CMakeLists.txt` for the project.
6. Write the `tasks.py`
    ```py
    """
    This requires invoke to be installed on the machine (not venv), which can be
    done via:
        yay -S python-invoke
    """
    from datetime import datetime
    from hashlib import md5
    from pathlib import Path
    from shutil import rmtree as shutil_rmtree
    from typing import Optional
    
    from invoke import task
    
    PROJECT: str = "chap0"
    SRC_PATH: Path = Path(__file__).parent
    VCPKG_TOOLCHAIN = SRC_PATH / "vcpkg/scripts/buildsystems/vcpkg.cmake"
    WORKSPACE: Path = Path("/tmp/builds/cpp")
    MD5: Optional[str] = None
    BUILD_PATH: Optional[Path] = Path(f"{SRC_PATH}/build")
    INSTALL_PATH: Optional[Path] = None
    
    
    def get_md5(content: str) -> str:
        global MD5
        if MD5 is None:
            MD5 = md5(str.encode(content)).hexdigest()
        return MD5
    
    
    def get_cmake_workspace() -> Path:
        hash = get_md5(str(SRC_PATH))
        return WORKSPACE / f"{PROJECT}_{SRC_PATH.name}_{hash}"
    
    
    def get_build_path() -> Path:
        global BUILD_PATH
        if BUILD_PATH is None:
            BUILD_PATH = get_cmake_workspace() / "build"
        return BUILD_PATH
    
    
    def get_install_path() -> Path:
        global INSTALL_PATH
        if INSTALL_PATH is None:
            INSTALL_PATH = get_cmake_workspace() / "install"
        return INSTALL_PATH
    
    
    @task
    def info(c, topic="all"):
        """Show project info."""
        if topic == "all":
            print(f"Project         = {PROJECT}")
            print(f"Source path     = {SRC_PATH}")
            print(f"Build path      = {get_build_path()}")
            print(f"Install path    = {get_install_path()}")
        elif topic == "build_path":
            print(get_build_path())
        elif topic == "install_path":
            print(get_install_path())
        else:
            print("Error: Valid 'topic' names are 'build_path'/'install_path'")
    
    
    @task
    def config(c):
        """Run Make configure."""
        do_config(c)
    
    
    def do_config(c):
        build_path = get_build_path()
        build_path.mkdir(parents=True, exist_ok=True)
        cmd = [
            "cmake",
            "-S",
            str(SRC_PATH),
            "-B",
            str(build_path),
            "-GNinja",
            "-DCMAKE_BUILD_TYPE=RelWithDebInfo",
            "-DCMAKE_EXPORT_COMPILE_COMMANDS=1",
            f"-DCMAKE_TOOLCHAIN_FILE={str(VCPKG_TOOLCHAIN)}",
        ]
        c.run(" ".join(cmd), pty=True)
    
        # Symlink compile_commands.json
        src_ccdb_file = SRC_PATH / "compile_commands.json"
        build_ccdb_file = build_path / "compile_commands.json"
        if build_ccdb_file.exists():
            if not src_ccdb_file.exists():
                src_ccdb_file.symlink_to(build_ccdb_file)
    
    
    @task
    def build(c, config=False):
        """Run builds via CMake."""
        build_path = get_build_path()
    
        if not build_path.exists():
            if config:
                do_config(c)
            else:
                print("Error: build path doesn't exist.")
                return
    
        cmd = ["cmake", "--build", str(build_path)]
        c.run(" ".join(cmd), pty=True)
    
    
    @task
    def install(c):
        """Run install via CMake."""
        build_path = get_build_path()
        install_path = get_install_path()
    
        if not build_path.exists():
            print("Error: build path doesn't exist.")
            return
    
        cmd = ["cmake", "--install", str(build_path)]
        c.run(" ".join(cmd), env={"DESTDIR": install_path}, pty=True)
    
    
    @task
    def clean(c):
        """Clean build directory."""
        # Don't clean during CppCon and the week before/after
        _, week, _ = datetime.now().isocalendar()
        if week in (36, 37, 38):
            print(f"I'm sorry I can't do that Dave as the current week is {week}.")
            return
    
        build_path = get_build_path()
        if build_path.exists():
            shutil_rmtree(build_path)
            print(f"Cleaned {build_path}")
        else:
            print("Build path absent. Nothing to do.")
    
    
    @task(pre=[clean])
    def clean_all(c):
        """Clean build and install directory."""
        install_path = get_install_path()
        if install_path.exists():
            shutil_rmtree(install_path)
            print(f"Cleaned {install_path}")
        else:
            print("Install path absent. Nothing to do.")
    
    
    @task
    def ls(c):
        """List files using lsd and skip vcpkg and .cache folder"""
        cmd = [
            "lsd",
            "--tree",
            "--ignore-glob vcpkg",
            "--ignore-glob .cache",
        ]
        c.run(" ".join(cmd), pty=True)
    ```
7. Run the Invoke Command to configure and build your project for first time.
    ```bash
    inv config build
    ```
    This step will generate the `build` directory and the `compile_commands.json` which will help with project intellisense.
8. Add `.clang-format` file in the root of the project. This file contains the guidelines to format the project. You can select the guidelines from one of the many poplular ones found [here](https://clang.llvm.org/docs/ClangFormat.html). Add one to your project as follows:
    ```bash
    clang-format -style=google -dump-config > .clang-format
    ```
9. Add `.clang-tidy` for Linting. It will help you provide diagnostics related to code errors, static analysis and code quality. Guide on clang-tidy can be found [here](https://clang.llvm.org/extra/clang-tidy/). It can be added to project as follows:
    ```clang-tidy
    ---
    # Enable ALL the things! Except not really
    # misc-non-private-member-variables-in-classes: the options don't do anything
    Checks: "*,\
      -google-readability-todo,\
      -altera-*,\
      -fuchsia-*,\
      fuchsia-multiple-inheritance,\
      -llvm-header-guard,\
      -llvm-include-order,\
      -llvmlibc-*,\
      -misc-non-private-member-variables-in-classes"
    WarningsAsErrors: ''
    CheckOptions:
      - key: 'bugprone-argument-comment.StrictMode'
        value: 'true'
    # Prefer using enum classes with 2 values for parameters instead of bools
      - key: 'bugprone-argument-comment.CommentBoolLiterals'
        value: 'true'
      - key: 'bugprone-misplaced-widening-cast.CheckImplicitCasts'
        value: 'true'
      - key: 'bugprone-sizeof-expression.WarnOnSizeOfIntegerExpression'
        value: 'true'
      - key: 'bugprone-suspicious-string-compare.WarnOnLogicalNotComparison'
        value: 'true'
      - key: 'readability-simplify-boolean-expr.ChainedConditionalReturn'
        value: 'true'
      - key: 'readability-simplify-boolean-expr.ChainedConditionalAssignment'
        value: 'true'
      - key: 'readability-uniqueptr-delete-release.PreferResetCall'
        value: 'true'
      - key: 'cppcoreguidelines-init-variables.MathHeader'
        value: '<cmath>'
      - key: 'cppcoreguidelines-narrowing-conversions.PedanticMode'
        value: 'true'
      - key: 'readability-else-after-return.WarnOnUnfixable'
        value: 'true'
      - key: 'readability-else-after-return.WarnOnConditionVariables'
        value: 'true'
      - key: 'readability-inconsistent-declaration-parameter-name.Strict'
        value: 'true'
      - key: 'readability-qualified-auto.AddConstToQualified'
        value: 'true'
      - key: 'readability-redundant-access-specifiers.CheckFirstDeclaration'
        value: 'true'
    # These seem to be the most common identifier styles
      - key: 'readability-identifier-naming.AbstractClassCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.ClassCase'
        value: 'CamelCase'
      - key: 'readability-identifier-naming.ClassConstantCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.ClassMemberCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.ClassMethodCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.ConstantCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.ConstantMemberCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.ConstantParameterCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.ConstantPointerParameterCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.ConstexprFunctionCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.ConstexprMethodCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.ConstexprVariableCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.EnumCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.EnumConstantCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.FunctionCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.GlobalConstantCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.GlobalConstantPointerCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.GlobalFunctionCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.GlobalPointerCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.GlobalVariableCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.InlineNamespaceCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.LocalConstantCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.LocalConstantPointerCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.LocalPointerCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.LocalVariableCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.MacroDefinitionCase'
        value: 'UPPER_CASE'
      - key: 'readability-identifier-naming.MemberCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.MethodCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.NamespaceCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.ParameterCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.ParameterPackCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.PointerParameterCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.PrivateMemberCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.PrivateMemberPrefix'
        value: 'm_'
      - key: 'readability-identifier-naming.PrivateMethodCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.ProtectedMemberCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.ProtectedMemberPrefix'
        value: 'm_'
      - key: 'readability-identifier-naming.ProtectedMethodCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.PublicMemberCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.PublicMethodCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.ScopedEnumConstantCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.StaticConstantCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.StaticVariableCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.StructCase'
        value: 'CamelCase'
      - key: 'readability-identifier-naming.TemplateParameterCase'
        value: 'CamelCase'
      - key: 'readability-identifier-naming.TemplateTemplateParameterCase'
        value: 'CamelCase'
      - key: 'readability-identifier-naming.TypeAliasCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.TypedefCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.TypeTemplateParameterCase'
        value: 'CamelCase'
      - key: 'readability-identifier-naming.UnionCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.ValueTemplateParameterCase'
        value: 'CamelCase'
      - key: 'readability-identifier-naming.VariableCase'
        value: 'lower_case'
      - key: 'readability-identifier-naming.VirtualMethodCase'
        value: 'lower_case'
    ...
    ```
10. Commit the project and push to github.
11. Exit the Neovim and restart again to trigger `clangd`. The `clangd` will look for `compile_commands.json` in the root of the project folder which will contain all the intellisense for the added packages and files.

---

## Usage

### Intellisense

For a correctly configured `lsp-zero` and `clangd`, the intellisense will be picked up automatically using the `compiled_commands.json` file.

### clang-format

Set the `lsp-zero`` to autoformat buffer on save to apply formatting on saving and make sure to have `.clang-format` file in root of the project directory.

### clang-tidy

You can either run `clang-tidy` manually or the `clangd` will automatically pick the `.clang-tidy` file from root of project and run it to the files on each buffer saves for linting and static analysis.

### Build

You can build the project by running:

```bash
inv build
```

This will save the build files in the folder `build/` and can be run from it.

## Resources
- [ C++ Coding with Neovim - Prateek Raman - CppCon 2022 ](https://youtu.be/nzRnWUjGJl8?si=3F7oa7XSnhV21EYV)
- [vcpkg Setup Guide](https://learn.microsoft.com/en-us/vcpkg/get_started/get-started?pivots=shell-bash)
- [clang-format](https://clang.llvm.org/docs/ClangFormat.html)
- [clang-tidy](https://clang.llvm.org/extra/clang-tidy/)
