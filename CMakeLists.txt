PROJECT(fuse-lwext4)
CMAKE_MINIMUM_REQUIRED(VERSION 2.8)

list(APPEND CMAKE_MODULE_PATH "${PROJECT_SOURCE_DIR}/cmake")

# Required software packages for this FUSE filesystem
FIND_PACKAGE(PkgConfig REQUIRED)
pkg_check_modules(FUSE REQUIRED fuse)
FIND_PACKAGE(lwext4 REQUIRED)

# Add the lwext4 and FUSE include directories...
INCLUDE_DIRECTORIES(${FUSE_INCLUDE_DIRS})
INCLUDE_DIRECTORIES(${LWEXT4_INCLUDE_DIR})

# Add a few additional flagvals for the C compile...
SET(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -D_FILE_OFFSET_BITS=64 -DFUSE_USE_VERSION=26 -std=gnu99 -g3 -pthread")

ADD_EXECUTABLE(fuse-lwext4 
    acl.c
    blockdev.c
    fuse-main.c 
    logging.c
    op_chmod.c 
    op_chown.c 
    op_create.c 
    op_destroy.c 
    op_getattr.c 
    op_init.c 
    op_link.c 
    op_mkdir.c 
    op_open.c 
    op_opendir.c 
    op_read.c 
    op_readdir.c 
    op_readlink.c 
    op_release.c 
    op_releasedir.c
    op_rename.c 
    op_rmdir.c 
    op_statvfs.c 
    op_symlink.c 
    op_truncate.c 
    op_unlink.c 
    op_utimens.c 
    op_write.c 
    op_xattr.c
)

TARGET_LINK_LIBRARIES(fuse-lwext4 ${FUSE_LIBRARIES} ${LWEXT4_LIBRARY} )
INSTALL(TARGETS fuse-lwext4 RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})
