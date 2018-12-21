# OpenMVG Build For iOS
1.

`$ git clone --recursive https://github.com/openMVG/openMVG.git`

2.
edit the file openMVG/src/third_party/vectorGraphics/CMakeLists.txt and comment the two last lines :

```
CMAKE_MINIMUM_REQUIRED(VERSION 2.6)

#ADD_EXECUTABLE(main_svgSample main.cpp)

#SET_PROPERTY(TARGET main_svgSample PROPERTY FOLDER OpenMVG/Samples)

```
3.
edit the file openMVG/src/CMakeLists.txt and comment this line:

`#AutodetectHostArchitecture()`

4.

`$ mkdir build`

5.

`$ cd build`

6.

```
$ cmake -GXcode -DCMAKE_BUILD_TYPE=Release \
		 -DOpenMVG_BUILD_EXAMPLES=OFF \
		 -DOpenMVG_BUILD_SOFTWARES=OFF \
		 -DOpenMVG_BUILD_GUI_SOFTWARES=OFF \
		 -DTARGET_ARCHITECTURE=generic  . ../openMVG/src/

```

7.

`$ open openMVG.xcodeproj`

8.

Set the build settings globally ( Base SDK and Build Active Architecture Only ) :

![Architecture pic](./ArchitecturePic.png)

