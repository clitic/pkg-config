project(
  'pkg-config',
  'c',
  version: '0.29.2',
  license: 'GPL-2.0',
  meson_version: '>=0.64.0'
)

cc = meson.get_compiler('c')
system = host_machine.system()

pkg_config_defines = []

system_include_path = get_option('system-include-path')

if system_include_path == 'auto' and system == 'windows'
  pkg_config_defines += '-DPKG_CONFIG_SYSTEM_INCLUDE_PATH=""'
elif system_include_path == 'auto'
  pkg_config_defines += '-DPKG_CONFIG_SYSTEM_INCLUDE_PATH="/usr/include:/usr/local/include"'
else
  pkg_config_defines += f'-DPKG_CONFIG_SYSTEM_INCLUDE_PATH="@system_include_path@"'
endif

system_library_path = get_option('system-library-path')

if system_library_path == 'auto' and system == 'windows'
  pkg_config_defines += '-DPKG_CONFIG_SYSTEM_LIBRARY_PATH=""'
elif system_library_path == 'auto'
  pkg_config_defines += '-DPKG_CONFIG_SYSTEM_LIBRARY_PATH="/usr/lib:/usr/local/lib"'
else
  pkg_config_defines += f'-DPKG_CONFIG_SYSTEM_LIBRARY_PATH="@system_library_path@"'
endif

pkg_config_defines += '-DENABLE_DEFINE_PREFIX=' + get_option('enable-define-prefix').to_int().to_string()
pkg_config_defines += '-DENABLE_INDIRECT_DEPS=' + get_option('enable-indirect-deps').to_int().to_string()

if cc.has_header('dirent.h')
  pkg_config_defines += '-DHAVE_DIRENT_H'
endif

if cc.has_header('dlfcn.h')
  pkg_config_defines += '-DHAVE_DLFCN_H'
endif

if cc.has_header('inttypes.h')
  pkg_config_defines += '-DHAVE_INTTYPES_H'
endif

if cc.has_header('malloc.h')
  pkg_config_defines += '-DHAVE_MALLOC_H'
endif

if cc.has_header('memory.h')
  pkg_config_defines += '-DHAVE_MEMORY_H'
endif

if cc.has_header('stdint.h')
  pkg_config_defines += '-DHAVE_STDINT_H'
endif

if cc.has_header('stdlib.h')
  pkg_config_defines += '-DHAVE_STDLIB_H'
endif

if cc.has_header('strings.h')
  pkg_config_defines += '-DHAVE_STRINGS_H'
endif

if cc.has_header('string.h')
  pkg_config_defines += '-DHAVE_STRING_H'
endif

if cc.has_header('sys/stat.h')
  pkg_config_defines += '-DHAVE_SYS_STAT_H'
endif

if cc.has_header('sys/types.h')
  pkg_config_defines += '-DHAVE_SYS_TYPES_H'
endif

if cc.has_header('sys/wait.h')
  pkg_config_defines += '-DHAVE_SYS_WAIT_H'
endif

if cc.has_header('unistd.h')
  pkg_config_defines += '-DHAVE_UNISTD_H'
endif

pkg_config_defines += '-DLT_OBJDIR=".libs/"'
pkg_config_defines += '-DPACKAGE="@0@"'.format(meson.project_name())
pkg_config_defines += '-DPACKAGE_BUGREPORT="https://gitlab.freedesktop.org/pkg-config/pkg-config"'
pkg_config_defines += '-DPACKAGE_NAME="@0@"'.format(meson.project_name())
pkg_config_defines += '-DPACKAGE_STRING="@0@ @1@"'.format(meson.project_name(), meson.project_version())
pkg_config_defines += '-DPACKAGE_TARNAME="@0@-@1@"'.format(meson.project_name(), meson.project_version())
pkg_config_defines += '-DPACKAGE_URL="https://gitlab.freedesktop.org/pkg-config/pkg-config"'
pkg_config_defines += '-DPACKAGE_VERSION="@0@"'.format(meson.project_version())

if cc.has_header('stdlib.h')
  pkg_config_defines += '-DSTDC_HEADERS'
endif

pkg_config_defines += '-DVERSION="@0@"'.format(meson.project_version())
pkg_config_defines += '-DDARWIN_USE_64_BIT_INODE=1'

libglib_dep = dependency('glib-2.0', required: false, static: get_option('static-glib'))

if not libglib_dep.found() or get_option('build-glib') == 'true'
  glib = subproject('glib', default_options: ['tests=false'])
  libglib_dep = glib.get_variable('libglib_dep')
endif

executable(
  'pkg-config',
  [
    'pkg-config/pkg.h',
    'pkg-config/pkg.c',
    'pkg-config/parse.h',
    'pkg-config/parse.c',
    'pkg-config/rpmvercmp.c',
    'pkg-config/rpmvercmp.h',
    'pkg-config/main.c'
  ],
  c_args: pkg_config_defines,
  dependencies: libglib_dep,
  install: true,
)

install_man('pkg-config/pkg-config.1')

install_data(
  configure_file(
    input: 'pkg-config/pkg.m4.in',
    output: 'pkg.m4',
    configuration: {
      'VERSION': meson.project_version()
    }
  ),
  install_dir: get_option('datadir') / 'aclocal'
)
