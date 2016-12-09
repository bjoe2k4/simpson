#
# CMake Build for SIMPSON
#
cmake_minimum_required (VERSION 2.6.8)

# default build mode
if (NOT CMAKE_BUILD_TYPE)
  set (CMAKE_BUILD_TYPE "RelWithDebInfo")
  message ("CMake build mode: RelWithDebInfo")
endif (NOT CMAKE_BUILD_TYPE)

project (SIMPSON C)

find_package (BLAS REQUIRED)
find_package (TCL REQUIRED)

include (ExternalProject)
ExternalProject_Add (nfft3
  PREFIX nfft3
  URL https://www-user.tu-chemnitz.de/~potts/nfft/download/nfft-3.3.2.tar.gz
  URL_HASH MD5=550737c06f4d6ea6c156800169d8f0d9
  CONFIGURE_COMMAND <SOURCE_DIR>/configure --prefix=<INSTALL_DIR>
  BUILD_COMMAND make
  INSTALL_COMMAND make install
  )
ExternalProject_Get_Property (nfft3 install_dir)
set (NFFT3_INCLUDE_DIRS "${install_dir}/include")
include_directories ("${NFFT3_INCLUDE_DIRS}")

add_executable (SIMPSON
  allocation.c
  auxmath.c
  averaging.c
  B0inhom.c
  blockdiag.c
  cm.c
  complx.c
  cryst.c
  crystdat.c
  distortions.c
  fft.c
  fidcalc.c
  ftools.c
  ham.c
  iodata.c
  isotopes.c
  lbfgs.c
  main.c
  matrix.c
  OCroutines.c
  pthread_barrier_mac.c
  pulse.c
  readsys.c
  relax.c
  rfprof.c
  rfshapes.c
  sim.c
  simpson.c
  spinach.c
  spinsys.c
  tclcode.c
  tclutil.c
  wigner.c)

target_link_libraries (SIMPSON ${TCL_LIBRARY})
add_dependencies (SIMPSON nfft3)
