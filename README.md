<h1 align="center">pkg-config</h1>

<p align="center">
  <a href="https://github.com/clitic/pkg-config">
    <img src="https://img.shields.io/github/downloads/clitic/pkg-config/total?style=flat-square">
  </a>
  <a href="https://github.com/clitic/pkg-config">
    <img src="https://img.shields.io/github/repo-size/clitic/pkg-config?logo=github&style=flat-square">
  </a>
</p>

[pkg-config](https://gitlab.freedesktop.org/pkg-config/pkg-config) is a script to make putting together all the build flags when compiling/linking a lot easier. Visit [releases](https://github.com/clitic/pkg-config/releases) for prebuilt binaries. Extract archive and add `bin` directory in your `PATH` environment variable.

## Building

1. Install [meson](https://mesonbuild.com/SimpleStart.html) build system. Then install any one of these c compiler: [msvc](https://visualstudio.microsoft.com), [gcc](https://gcc.gnu.org), [mingw](https://www.mingw-w64.org/downloads) or [clang](https://github.com/llvm/llvm-project).

2. Now recursively clone pkg-config repository.

```bash
git clone https://github.com/clitic/pkg-config --recursive --depth 1
cd pkg-config
```

3. Now setup pkg-config using meson. Use `--prefix` flag to specify installation directory. For more meson options see [meson_options.txt](https://github.com/clitic/pkg-config/blob/main/meson_options.txt).

```bash
meson setup build --buildtype release --warnlevel 0 --default-library static --prefix c:/pkg-config -Dbuild-glib=true
```

4. Now install pkg-config using meson. The skipped subprojects are not needed at runtime if you still want to keep them, then remove `--skip-subprojects` flag.

```bash
meson install -C build --skip-subprojects "glib,libffi,pcre2,proxy-libintl,zlib"
```

5. Now pkg-config should be installed in your prefix directory. Now add `$PREFIX/bin` in your `PATH` environment variable. You can further read more information about building through meson from meson's quick [guide](https://mesonbuild.com/Quick-guide.html).
