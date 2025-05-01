project('adjust', 'cpp',
  default_options: ['buildtype=release', 'warning_level=2', 'b_lto=true', 'b_ndebug=if-release', 'cpp_std=c++11'],
  license: 'GPL-3.0-or-later',
  meson_version: '>=0.51.0',
)

cxx = meson.get_compiler('cpp')

gcc_syntax = cxx.get_argument_syntax() == 'gcc'

add_project_arguments(['-fopenmp'], language: 'cpp')

if gcc_syntax
  vapoursynth_dep = dependency('vapoursynth', version: '>=55').partial_dependency(compile_args: true, includes: true)
  install_dir = vapoursynth_dep.get_variable(pkgconfig: 'libdir') / 'vapoursynth'
else
  # MSVC
  vs_sdk = get_option('vs_sdk')
  if vs_sdk == ''
    error('Cannot find VapourSynth SDK. Please set vs_sdk option.')
  endif
  
  vs_include = vs_sdk + '/sdk/include/vapoursynth'
  vs_lib_dir = vs_sdk + '/sdk/lib64'

  vs_lib = cxx.find_library('vapoursynth', dirs: [vs_lib_dir])
  
  vs_include_args = ['-I' + vs_include]
  
  vapoursynth_dep = declare_dependency(
    compile_args: vs_include_args,
    dependencies: [vs_lib]
  )
  
  install_dir = vs_sdk + '/plugins'
endif

sources = [
  'adjust/adjust.cpp'
]

shared_module('adjust', sources,
  dependencies: [vapoursynth_dep],
  install: true,
  install_dir: install_dir,
  gnu_symbol_visibility: 'hidden'
)