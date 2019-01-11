# OpenMVG Build For iOS

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
edit the file src/openMVG/graph/graph_graphviz_export.hpp and comment the following three lines :

```
  //Use Graphviz
  //const std::string cmd = "neato -Tsvg -O -Goverlap=scale -Gsplines=false " + sfile;
  //const int ret = std::system(cmd.c_str());
  //(void)ret;
```

4.
`$ mkdir build`

5.
`$ cd build`

6.

```
$ cmake -GXcode -DCMAKE_BUILD_TYPE=Release \
		 -DCMAKE_OSX_SYSROOT=''				   \
		 -DTARGET_ARCHITECTURE=generic	   \
		 -DOpenMVG_BUILD_DOC=OFF			   \
		 -DOpenMVG_BUILD_EXAMPLES=OFF 	   \
		 -DOpenMVG_BUILD_SOFTWARES=OFF 	   \
		 -DOpenMVG_BUILD_GUI_SOFTWARES=OFF  . ../openMVG/src/
```

7.
`$ open openMVG.xcodeproj`

8.
Set the build settings globally ( Base SDK and Build Active Architecture Only ) :

![Architecture pic](https://github.com/weifud/UnimportantItem/blob/master/Image/ArchitecturePic.png)


