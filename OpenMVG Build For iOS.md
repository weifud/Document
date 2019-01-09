# OpenMVG_4DAGE Build For iOS

1.
edit the file src/third_party/vectorGraphics/CMakeLists.txt and comment the two last lines :

```
CMAKE_MINIMUM_REQUIRED(VERSION 2.6)

#ADD_EXECUTABLE(main_svgSample main.cpp)

#SET_PROPERTY(TARGET main_svgSample PROPERTY FOLDER OpenMVG/Samples)

```
2.
edit the file src/CMakeLists.txt and comment this line:

`#AutodetectHostArchitecture()`

3.
edit the file src/CMakeLists.txt:

```
# ==============================================================================
# IMAGE IO detection
# ==============================================================================
set(CMAKE_FIND_FRAMEWORK LAST)
#find_package(JPEG QUIET)
#find_package(PNG QUIET)
#find_package(TIFF QUIET)
# ==============================================================================
#- jpeg
# ==============================================================================
# - internal by default (jpeg),
# - external if JPEG_INCLUDE_DIR_HINTS and a valid jpeg setup is found
# ==============================================================================
if (NOT DEFINED JPEG_INCLUDE_DIR_HINTS)
set(JPEG_INCLUDE_DIR_HINTS ${CMAKE_CURRENT_SOURCE_DIR}/third_party/jpeg)
set(OpenMVG_USE_INTERNAL_JPEG ON)
endif()
find_package(JPEG QUIET)
if (NOT JPEG_FOUND OR OpenMVG_USE_INTERNAL_JPEG)
set(JPEG_INCLUDE_DIRS ${CMAKE_CURRENT_SOURCE_DIR}/third_party/jpeg)
endif()

# ==============================================================================
#- png
# ==============================================================================
# - internal by default (png),
# - external if PNG_INCLUDE_DIR_HINTS and a valid png setup is found
# ==============================================================================
if (NOT DEFINED PNG_INCLUDE_DIR_HINTS)
set(PNG_INCLUDE_DIR_HINTS ${CMAKE_CURRENT_SOURCE_DIR}/third_party/png)
set(OpenMVG_USE_INTERNAL_PNG ON)
endif()
find_package(PNG QUIET)
if (NOT PNG_FOUND OR OpenMVG_USE_INTERNAL_PNG)
set(PNG_INCLUDE_DIRS ${CMAKE_CURRENT_SOURCE_DIR}/third_party/png)
endif()

# ==============================================================================
#- tiff
# ==============================================================================
# - internal by default (tiff),
# - external if TIFF_INCLUDE_DIR_HINTS and a valid TIFF setup is found
# ==============================================================================
if (NOT DEFINED TIFF_INCLUDE_DIR_HINTS)
set(TIFF_INCLUDE_DIR_HINTS ${CMAKE_CURRENT_SOURCE_DIR}/third_party/tiff)
set(OpenMVG_USE_INTERNAL_TIFF ON)
endif()
find_package(TIFF QUIET)
if (NOT TIFF_FOUND OR OpenMVG_USE_INTERNAL_TIFF)
set(TIFF_INCLUDE_DIRS ${CMAKE_CURRENT_SOURCE_DIR}/third_party/tiff)
endif()

# ==============================================================================
# --END-- IMAGE IO detection
# ==============================================================================
```
4.
edit the file src/third_party/CMakeLists.txt:

```
##
# Image I/O
##
#if(NOT JPEG_FOUND)
#  set(OpenMVG_USE_INTERNAL_JPEG ON PARENT_SCOPE)
if (DEFINED OpenMVG_USE_INTERNAL_JPEG)
  set(JPEG_INCLUDE_INSTALL_DIR ${CMAKE_INSTALL_PREFIX}/include/openMVG/third_party/jpeg)
  add_subdirectory(jpeg)
  set_property(TARGET jpeg PROPERTY FOLDER OpenMVG/3rdParty/jpeg)
  list(APPEND JPEG_INCLUDE_DIRECTORIES ${CMAKE_CURRENT_SOURCE_DIR}/jpeg ${CMAKE_CURRENT_BINARY_DIR}/jpeg/config)
  set(JPEG_INCLUDE_DIR ${JPEG_INCLUDE_DIRECTORIES})
  set(JPEG_LIBRARIES jpeg PARENT_SCOPE)
  set(JPEG_INCLUDE_DIR ${JPEG_INCLUDE_DIR} PARENT_SCOPE)
endif()
#endif(NOT JPEG_FOUND)

# TIFF and PNG depend on zlib, if one of them is not found add the internal zlib
#if(NOT PNG_FOUND OR NOT TIFF_FOUND)
  set(ZLIB_INCLUDE_INSTALL_DIR ${CMAKE_INSTALL_PREFIX}/include/openMVG/third_party/zlib)
  add_subdirectory(zlib)
  set_property(TARGET zlib PROPERTY FOLDER OpenMVG/3rdParty/zlib)
#endif(NOT PNG_FOUND OR NOT TIFF_FOUND)

#if (NOT PNG_FOUND)
#  set(OpenMVG_USE_INTERNAL_PNG ON PARENT_SCOPE)
if (DEFINED OpenMVG_USE_INTERNAL_PNG)
  set(PNG_INCLUDE_INSTALL_DIR ${CMAKE_INSTALL_PREFIX}/include/openMVG/third_party/png)
  add_subdirectory(png)
  set_property(TARGET png PROPERTY FOLDER OpenMVG/3rdParty/png)
  set(PNG_LIBRARIES png zlib)
  list(APPEND PNG_INCLUDE_DIRECTORIES ${CMAKE_CURRENT_SOURCE_DIR}/png ${CMAKE_CURRENT_SOURCE_DIR}/zlib ${CMAKE_CURRENT_BINARY_DIR}/png/config)
  set(PNG_INCLUDE_DIRS ${PNG_INCLUDE_DIRECTORIES})
  set(PNG_LIBRARIES ${PNG_LIBRARIES} PARENT_SCOPE)
  set(PNG_INCLUDE_DIRS ${PNG_INCLUDE_DIRS} PARENT_SCOPE)
endif()
#endif (NOT PNG_FOUND)

#if (NOT TIFF_FOUND)
#  set(OpenMVG_USE_INTERNAL_TIFF ON PARENT_SCOPE)
if (DEFINED OpenMVG_USE_INTERNAL_TIFF)
  set(TIFF_INCLUDE_INSTALL_DIR ${CMAKE_INSTALL_PREFIX}/include/openMVG/third_party/tiff)
  add_subdirectory(tiff)
  set_property(TARGET tiff PROPERTY FOLDER OpenMVG/3rdParty/tiff)
  list(APPEND TIFF_INCLUDE_DIRECTORIES ${CMAKE_CURRENT_SOURCE_DIR}/tiff ${CMAKE_CURRENT_BINARY_DIR}/tiff)
  set(TIFF_INCLUDE_DIR ${TIFF_INCLUDE_DIRECTORIES})
  set(TIFF_LIBRARIES tiff)
  set(TIFF_LIBRARIES ${TIFF_LIBRARIES} PARENT_SCOPE)
  set(TIFF_INCLUDE_DIR ${TIFF_INCLUDE_DIR} PARENT_SCOPE)
endif()
#endif (NOT TIFF_FOUND)
##
# End - Image I/O
##
```
5.
edit the file src/openMVG/graph/graph_graphviz_export.hpp and comment the following three lines :

```
  //Use Graphviz
  //const std::string cmd = "neato -Tsvg -O -Goverlap=scale -Gsplines=false " + sfile;
  //const int ret = std::system(cmd.c_str());
  //(void)ret;
```

6.
`$ mkdir build`

7.
`$ cd build`

8.

```
$ cmake -GXcode -DCMAKE_BUILD_TYPE=Release \
		 -DCMAKE_OSX_SYSROOT=''				   \
		 -DTARGET_ARCHITECTURE=generic	   \
		 -DOpenMVG_BUILD_DOC=OFF			   \
		 -DOpenMVG_BUILD_EXAMPLES=OFF 	   \
		 -DOpenMVG_BUILD_SOFTWARES=OFF 	   \
		 -DOpenMVG_BUILD_GUI_SOFTWARES=OFF  . ../openMVG/src/
```

9.
`$ open openMVG.xcodeproj`

10.
Set the build settings globally ( Base SDK and Build Active Architecture Only ) :

![Architecture pic](https://github.com/weifud/UnimportantItem/blob/master/Image/ArchitecturePic.png)


