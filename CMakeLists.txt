cmake_minimum_required(VERSION 3.2)

project(libnfs
        LANGUAGES C
        VERSION 4.0.0)

add_library(nfs
  lib/init.c
  lib/libnfs-sync.c
  lib/libnfs-zdr.c
  lib/libnfs.c
  lib/nfs_v3.c
  lib/nfs_v4.c
  lib/pdu.c
  lib/socket.c
  include/nfsc/libnfs.h
  include/nfsc/libnfs-raw.h
  include/nfsc/libnfs-zdr.h
  include/win32/win32_compat.h
  include/libnfs-private.h
  mount/libnfs-raw-mount.h
  mount/libnfs-raw-mount.c
  mount/mount.c
  nfs/libnfs-raw-nfs.h
  nfs/libnfs-raw-nfs.c
  nfs/nfs.c
  nfs/nfsacl.c
  nfs4/libnfs-raw-nfs4.c
  nfs4/nfs4.c
  nlm/libnfs-raw-nlm.c
  nlm/libnfs-raw-nlm.h
  nlm/nlm.c
  nsm/libnfs-raw-nsm.c
  nsm/libnfs-raw-nsm.h
  nsm/nsm.c
  portmap/libnfs-raw-portmap.h
  portmap/libnfs-raw-portmap.c
  portmap/portmap.c
  rquota/libnfs-raw-rquota.h
  rquota/libnfs-raw-rquota.c
  rquota/rquota.c
  win32/win32_compat.c
  win32/win32_errnowrapper.h
)


target_include_directories(
  nfs PRIVATE
  $<BUILD_INTERFACE:include/;include/nfsc/;win32/;rquota/;nsm/;nlm/;nfs/;mount/;lib/;portmap/>
  INTERFACE
  $<INSTALL_INTERFACE:include/nfsc>
)

target_compile_definitions(nfs PRIVATE _U_=  _CRT_NONSTDC_NO_DEPRECATE _CRT_SECURE_NO_WARNINGS)
target_link_libraries(nfs PRIVATE ws2_32.lib)
set_target_properties(nfs PROPERTIES COMPILE_PDB_OUTPUT_DIRECTORY ${PROJECT_BINARY_DIR} COMPILE_PDB_NAME nfs)

install(TARGETS nfs EXPORT nfs
  RUNTIME DESTINATION bin
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib)


install(DIRECTORY
  include/nfsc
  DESTINATION include
)

install(FILES
  mount/libnfs-raw-mount.h
  nfs/libnfs-raw-nfs.h
  nlm/libnfs-raw-nlm.h
  nsm/libnfs-raw-nsm.h
  portmap/libnfs-raw-portmap.h
  rquota/libnfs-raw-rquota.h
  DESTINATION include/nfsc
)

if(MSVC)
  install(FILES "${CMAKE_BINARY_DIR}/RelWithDebInfo/nfs.pdb" DESTINATION lib CONFIGURATIONS RelWithDebInfo)
endif()
