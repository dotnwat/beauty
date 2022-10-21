project('beauty', 'cpp',
  version: '1.0.0-rc2',
  meson_version: '>=0.63.0',
  default_options : [
    'cpp_std=c++20',
    'warning_level=1',
    'default_library=shared'])

version_h = configure_file(
  input: 'src/version.hpp.in',
  output: 'version.hpp',
  configuration: {'VERSION': meson.project_version()},
  format: 'cmake')

beauty_dep = library('beaty',
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
    include_directories: 'include')

#    find_package(Boost REQUIRED)
#    if (BEAUTY_ENABLE_OPENSSL)
#        find_package(OpenSSL REQUIRED)
#        add_library(openssl::openssl ALIAS OpenSSL::SSL)
#    endif()
