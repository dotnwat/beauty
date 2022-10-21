project('beauty', 'cpp',
  version: '1.0.0-rc2',
  meson_version: '>=0.63.0',
  default_options : [
    'cpp_std=c++20',
    'warning_level=1',
    'default_library=shared'])

subdir('src/beauty')

beauty_dep = library('beauty',
    'src/acceptor.cpp',
    'src/application.cpp',
    'src/attributes.cpp',
    'src/client.cpp',
    'src/exception.cpp',
    'src/route.cpp',
    'src/router.cpp',
    'src/server.cpp',
    'src/signal.cpp',
    'src/swagger.cpp',
    'src/timer.cpp',
    'src/url.cpp',
    'src/utils.cpp',
    version_h,
    include_directories: ['include', 'src'])
