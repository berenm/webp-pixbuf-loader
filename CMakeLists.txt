cmake_minimum_required(VERSION 3.6)

project(gdk-pixbuf-loader-webp)

include(GNUInstallDirs)
find_package(PkgConfig)

add_library(pixbufloader-webp SHARED
  io-webp.c
)

pkg_check_modules(WEBP REQUIRED libwebp>=0.4.3)
target_include_directories(pixbufloader-webp PUBLIC ${WEBP_INCLUDE_DIRS})
target_link_libraries(pixbufloader-webp PUBLIC ${WEBP_LIBRARIES})
link_directories(${WEBP_LIBRARY_DIRS})

pkg_check_modules(GDK_PIXBUF REQUIRED gdk-pixbuf-2.0)
target_include_directories(pixbufloader-webp PUBLIC ${GDK_PIXBUF_INCLUDE_DIRS})
target_link_libraries(pixbufloader-webp PUBLIC ${GDK_PIXBUF_LIBRARIES})
link_directories(${GDK_PIXBUF_LIBRARY_DIRS})

if ($ENV{GDK_PIXBUF_MODULEDIR})
  set(GDK_PIXBUF_MODULEDIR $ENV{GDK_PIXBUF_MODULEDIR})
else()
  pkg_get_variable(GDK_PIXBUF_BINARY_VERSION gdk-pixbuf-2.0 gdk_pixbuf_binary_version)
  set(GDK_PIXBUF_MODULEDIR "${CMAKE_INSTALL_LIBDIR}/gdk-pixbuf-2.0/${GDK_PIXBUF_BINARY_VERSION}/loaders")
endif()

install(TARGETS pixbufloader-webp LIBRARY DESTINATION "${GDK_PIXBUF_MODULEDIR}")
