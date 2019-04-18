# VS2017+Qt5.12.0+PCL1.9.1开发环境配置过程记录
## 系统环境说明
Windows 10 64位

## VS2017安装
Visual Studio 2017采用Community（社区）版本，可前往微软官网下载：[Visual Studio官方下载地址](https://visualstudio.microsoft.com/zh-hans/downloads/)，可根据个人喜好选择安装路径，本人的安装路径为**D:\ProgramFile\VS2017**，仅供参考。

## Qt5.12.0安装
Qt5.12.0可前往Qt官网下载[Qt官方下载地址](https://www.qt.io/download)，选择的是Open Source版本。安装路径可依个人喜好选择，本人的安装路径为**C:\Qt\Qt5.12.0**，仅供参考。

## 在VS2017上实现搭建Qt环境
这需要在VS中，通过“工具->扩展和更新->联机->Qt Visual Studio Tools”，安装Qt Visual Studio Tools，并进行相关配置，网上有大量相关教程，此处不再赘述。

## PCL1.9.1安装
### 1.下载
Point Cloud Library （PCL）可以在其Github网站上的发布页获取Windows下的安装包，链接如下：[https://github.com/PointCloudLibrary/pcl/releases/tag/pcl-1.9.1](https://github.com/PointCloudLibrary/pcl/releases/tag/pcl-1.9.1)
根据自己的**编译器环境**下载win64或者win32版本，本人下载的是64位版本的安装文件，即**PCL-1.9.1-AllInOne-msvc2017-win64.exe**和**pcl-1.9.1-pdb-msvc2017-win64.zip**两个文件。GitHub下载页面如下面两幅图所示：![GitHub页面](https://img-blog.csdnimg.cn/20190417162255390.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9sb25nMzYx,size_16,color_FFFFFF,t_70)
![GitHub下载页面](https://img-blog.csdnimg.cn/20190417162342747.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9sb25nMzYx,size_16,color_FFFFFF,t_70)
### 2.安装
首先双击文件“PCL-1.9.0-AllInOne-msvc2017-win64.exe”进行安装，注意下面这一步建议选择第二个“Add PCL to the system PATH for all users”，它可以自动添加系统路径，不过也可以安装结束后自行添加。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417172252763.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9sb25nMzYx,size_16,color_FFFFFF,t_70)
选择安装路径时，如果选择默认文件夹“**C:\Program Files\PCL 1.9.1**”，则在VS项目属性中可以直接借用本人采用的项目属性配置文件，如果选择其他路径，也可以根据后面的讲解自行修改。![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417172541947.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9sb25nMzYx,size_16,color_FFFFFF,t_70)

后面的步骤建议全部采用默认设置。安装快结束时会弹出安装第三方库OpenNI的提示，这里也可以全部采用默认安装设置，这样后续的VS项目属性配置中可以借用本人采用的属性配置文件，本人的OpenNI安装路径如下：C:\Program Files\OpenNI2。
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019041717273291.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9sb25nMzYx,size_16,color_FFFFFF,t_70)

OpenNI安装完之后点击确定后，PCL也就很快安装成功了。然后需要解压文件“pcl-1.9.1-pdb-msvc2017-win64.zip”，将解压得到的文件夹中的多个pdb文件（程序数据库文件）添加到PCL安装目录，如：**C:\Program Files\PCL 1.9.1\bin**。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417172904843.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9sb25nMzYx,size_16,color_FFFFFF,t_70)
PCL的安装到此结束。
## 重新安装VTK为其添加Qt支持
要想使用Qt为PCL项目制作UI，需要让Qt能够调用VTK生成的QVTKWidget组件，而“QVTKWidget”只有在VTK编译时添加了Qt之后才会生成，“PCL-1.9.1-AllInOne-msvc2017-win64.exe”安装包里的VTK是没有编译Qt支持的。要解决这个问题，可以在本地利用CMake重新编译VTK，并为其添加Qt支持。这一过程需要本地已经安装好Qt。
### 1.下载对应版本的VTK源码
PCL 1.9.1版本对应的VTK版本为8.1，VTK 8.1.0版本的源码可以通过其GitLab主页进行下载，[https://gitlab.kitware.com/vtk/vtk/tree/v8.1.0](https://gitlab.kitware.com/vtk/vtk/tree/v8.1.0)，下载页面如下所示，选择“Download zip”,即可得到得到源代码压缩文件“vtk-v8.1.0.zip”。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417173223754.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9sb25nMzYx,size_16,color_FFFFFF,t_70)
在合适的目录建立3个文件夹，分别命名为：
1.vtk_SRC:将VTK的源码存放在此处，作为CMake的代码源路径
2.vtk_build：CMake创建工程的目标路径
3.vtk_install：编译后版本的发布路径

本人将上述3个目录建立在路径“**D:\VTK_8_1_0**”下，其中vtk_SRC目录如下图所示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417210547415.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9sb25nMzYx,size_16,color_FFFFFF,t_70)
如果采用同样的目录进行vtk的安装，则在VS项目属性中可以直接借用本人采用的属性配置文件，如果选择其他路径，也可以根据后面的讲解自行修改。


## 2.下载安装Cmake

可以在[CMake官网](https://cmake.org/download/)下载合适的Windows安装程序，对于64位Windows系统，可以进行如下选择：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417210630207.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9sb25nMzYx,size_16,color_FFFFFF,t_70)
按照默认设置安装即可。

### 3.利用CMake生成VS工程
打开CMake GUI，设置源路径为vtk_SRC，目标路径为vtk_build，如下所示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417212327699.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9sb25nMzYx,size_16,color_FFFFFF,t_70)
初次配置，需要选择合适的编译器，点击左下角的"Configuration"按钮，选择编译器为“Visual Studio 15 2017”，编译器平台为“x64”：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417212340893.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9sb25nMzYx,size_16,color_FFFFFF,t_70)
点击“Add Entry”按钮，添加CMAKE_PREFIX_PATH，类型为“PATH”，其值设置为QT的安装路径，如“C:/Qt/Qt5.12.0/5.12.0/msvc2017_64”：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417213213593.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9sb25nMzYx,size_16,color_FFFFFF,t_70)
点击Add Entry，添加CMAKE_DEBUG_POSTFIX，类型为“STRING”，其值设置为"-gd"。用来区分debug与release版本下的dll和lib文件，不然的话创建安装文件的时候debug与release版本，名字相同相互覆盖。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417213259850.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9sb25nMzYx,size_16,color_FFFFFF,t_70)
勾选已有的“BUILD_SHARED_LIBIRARES”和“VTK_GROUP_QT”，勾选之后分别表示动态编译VTK以及给VTK添加Qt支持。
将“CMAKE_INSTALL_PREFIX”设置为“vtk_install”的路径。默认的路径在C盘的Programfiles里面，这可能需要管理员权限才能在默认路径下发布。
再次执行CMake配置，点击"Configuration"按钮，配置完成后，CMake界面显示如下所示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417213422761.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9sb25nMzYx,size_16,color_FFFFFF,t_70)
由此可见，CMake已经添加了所需的Qt相关目录，这里我们需要将"VTK_QT_VESION"设置为5。如果CMake没有找到QT5的路径，需要再次设置。
再次配置，点击"Configuration"按钮，界面下方显示"Configuring Done"则表示配置完成。这时可点击"Generate"按钮，生成VTK项目的Visual Studio工程。界面下方显示“Generating done”，即可在vtk_build目录中生成VTK项目的VS工程：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417214036975.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9sb25nMzYx,size_16,color_FFFFFF,t_70)

### 2.在VS中编译VTK
在VS2017中打开vtk_build目录中的“VTK.sln”文件，依次在“Debug-x64”和“Release-x64”编译环境下执行下列操作：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417214719734.png)![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417214728104.png)
在“解决方案资源管理器”中选择“ALL_BUILD”项目，鼠标右键单击后，选择“重新生成”，这一步编译需要的时间较长。然后再选中项目“INSTALL”，鼠标右击，依次选择“仅用于项目->仅生成INSTALL”，这一步编译所需时间较短。
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019041721432759.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9sb25nMzYx,size_16,color_FFFFFF,t_70)
分别在“Debug-x64”和“Release-x64”编译环境下执行上述操作之后，即可在vtk_install目录中得到vtk的完整安装结果。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417215408478.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9sb25nMzYx,size_16,color_FFFFFF,t_70)
至此，在"…\VTK_Install\ plugins\designer"的路径下，就可以发现文件"QVTKWidgetPlugin.dll"（Release生成）和“QVTKWidgetPlugin-gd.dll”（Debug生成）了。将Release生成的文件"QVTKWidgetPlugin.dll"复制到Qt的msvc2017_64版本的designer路径下，如"C:\Qt\Qt5.12.0\5.12.0\msvc2017_64\plugins\designer"，就可以在msvc2017_64版本的Qt Designer中添加QVTKWidget控件：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417220547763.png)
## VS2017+Qt5.12.0配置PCL1.9.1
### 1.系统环境变量配置
要想在VS+Qt开发环境中成功使用PCL1.9.1，首先要为PCL及其调用的第三方库添加系统环境变量。鼠标右键单机“计算机”，依次选择“属性->高级系统设置->环境变量”，双击打开“系统变量”中的"path"变量：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417210716181.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9sb25nMzYx,size_16,color_FFFFFF,t_70)
在path环境变量中添加下图红色方框中的部分：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417220902464.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9sb25nMzYx,size_16,color_FFFFFF,t_70)
环境变量配置完成之后需要将电脑注销一下才能生效（不用重启）。
### 2.配置相关项目属性
关于相关项目属性的配置，本文先借助一个空项目进行介绍，下一节将以PLC官方的Qt例程为例进行进一步的说明。
本文推荐使用新建属性表的方式管理项目属性，这样可以配置多个版本，需要时直接添加对应的属性表即可。若将属性配置全部添加在项目自带的属性表中，当以后添加的版本太多或者添加的其他库太多可能会导致配置属性冲突等后果。
首先在VS中新建一个Qt GUI空项目。![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418090825698.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9sb25nMzYx,size_16,color_FFFFFF,t_70)

针对“Debug_x64”编译环境，则在对应的“属性管理器->Debug|x64"中鼠标右击添加新项目属性表“PropertySheet1.props”（名字可以自行更改）。![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418092714731.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9sb25nMzYx,size_16,color_FFFFFF,t_70)
对于其他编译环境，均可以在属性管理器中添加相应的项目属性表，此处不再赘述。
#### 添加包含目录
在属性管理器中双击打开项目属性表“PropertySheet1.props”，选择“**通用属性->VC++目录->包含目录**”进行**编辑**，按照上述安装路径，添加右侧红色方框中的7个目录：![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418093434598.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9sb25nMzYx,size_16,color_FFFFFF,t_70)

```
D:\VTK_8_1_0\vtk_install\include\vtk-8.1
C:\Program Files\OpenNI2\Include
C:\Program Files\PCL 1.9.1\3rdParty\Qhull\include
C:\Program Files\PCL 1.9.1\3rdParty\FLANN\include
C:\Program Files\PCL 1.9.1\3rdParty\Eigen\eigen3
C:\Program Files\PCL 1.9.1\3rdParty\Boost\include\boost-1_68
C:\Program Files\PCL 1.9.1\include\pcl-1.9
```

#### 添加库目录
选择“**通用属性->VC++目录->库目录**”进行**编辑**，添加右侧红色方框中的6个目录：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418094059713.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9sb25nMzYx,size_16,color_FFFFFF,t_70)

```
D:\VTK_8_1_0\vtk_install\lib
C:\Program Files\PCL 1.9.1\3rdParty\Qhull\lib
C:\Program Files\OpenNI2\Lib
C:\Program Files\PCL 1.9.1\3rdParty\FLANN\lib
C:\Program Files\PCL 1.9.1\3rdParty\Boost\lib
C:\Program Files\PCL 1.9.1\lib
```

#### 添加预处理器定义
选择“**通用属性->C/C++->预处理器->预处理器定义**”进行**编辑**，添加右侧红色方框中的3个定义：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418094420736.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9sb25nMzYx,size_16,color_FFFFFF,t_70)

```
_CRT_SECURE_NO_WARNINGS
_SCL_SECURE_NO_WARNINGS
_SILENCE_FPOS_SEEKPOS_DEPRECATION_WARNING
```

#### 设置SDL检查
依次选择“通用属性->C/C++->所有选项->SDL检查”，将其设置为“**否（/sdl-）**”

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019041809445091.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9sb25nMzYx,size_16,color_FFFFFF,t_70)
#### 添加附加依赖项
依次选择“通用属性->链接器->输入->附加依赖项”进行“**编辑**”，并添加右侧红色方框中的“.lib文件”。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418094735474.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9sb25nMzYx,size_16,color_FFFFFF,t_70)
这里主要是添加PCL的“.lib”文件和VTK的".lib"文件，而且Debug编译环境和Release编译环境各自有对应的lib文件，本人在Debug编译环境下添加的附加依赖项如下：

```
pcl_common_debug.lib
pcl_features_debug.lib
pcl_filters_debug.lib
pcl_io_debug.lib
pcl_io_ply_debug.lib
pcl_kdtree_debug.lib
pcl_keypoints_debug.lib
pcl_ml_debug.lib
pcl_octree_debug.lib
pcl_outofcore_debug.lib
pcl_people_debug.lib
pcl_recognition_debug.lib
pcl_registration_debug.lib
pcl_sample_consensus_debug.lib
pcl_search_debug.lib
pcl_segmentation_debug.lib
pcl_stereo_debug.lib
pcl_surface_debug.lib
pcl_tracking_debug.lib
pcl_visualization_debug.lib
vtkalglib-8.1-gd.lib
vtkChartsCore-8.1-gd.lib
vtkCommonColor-8.1-gd.lib
vtkCommonComputationalGeometry-8.1-gd.lib
vtkCommonCore-8.1-gd.lib
vtkCommonDataModel-8.1-gd.lib
vtkCommonExecutionModel-8.1-gd.lib
vtkCommonMath-8.1-gd.lib
vtkCommonMisc-8.1-gd.lib
vtkCommonSystem-8.1-gd.lib
vtkCommonTransforms-8.1-gd.lib
vtkDICOMParser-8.1-gd.lib
vtkDomainsChemistry-8.1-gd.lib
vtkDomainsChemistryOpenGL2-8.1-gd.lib
vtkexoIIc-8.1-gd.lib
vtkexpat-8.1-gd.lib
vtkFiltersAMR-8.1-gd.lib
vtkFiltersCore-8.1-gd.lib
vtkFiltersExtraction-8.1-gd.lib
vtkFiltersFlowPaths-8.1-gd.lib
vtkFiltersGeneral-8.1-gd.lib
vtkFiltersGeneric-8.1-gd.lib
vtkFiltersGeometry-8.1-gd.lib
vtkFiltersHybrid-8.1-gd.lib
vtkFiltersHyperTree-8.1-gd.lib
vtkFiltersImaging-8.1-gd.lib
vtkFiltersModeling-8.1-gd.lib
vtkFiltersParallel-8.1-gd.lib
vtkFiltersParallelImaging-8.1-gd.lib
vtkFiltersPoints-8.1-gd.lib
vtkFiltersProgrammable-8.1-gd.lib
vtkFiltersSelection-8.1-gd.lib
vtkFiltersSMP-8.1-gd.lib
vtkFiltersSources-8.1-gd.lib
vtkFiltersStatistics-8.1-gd.lib
vtkFiltersTexture-8.1-gd.lib
vtkFiltersTopology-8.1-gd.lib
vtkFiltersVerdict-8.1-gd.lib
vtkfreetype-8.1-gd.lib
vtkGeovisCore-8.1-gd.lib
vtkgl2ps-8.1-gd.lib
vtkglew-8.1-gd.lib
vtkGUISupportQt-8.1-gd.lib
vtkGUISupportQtSQL-8.1-gd.lib
vtkhdf5-8.1-gd.lib
vtkhdf5_hl-8.1-gd.lib
vtkImagingColor-8.1-gd.lib
vtkImagingCore-8.1-gd.lib
vtkImagingFourier-8.1-gd.lib
vtkImagingGeneral-8.1-gd.lib
vtkImagingHybrid-8.1-gd.lib
vtkImagingMath-8.1-gd.lib
vtkImagingMorphological-8.1-gd.lib
vtkImagingSources-8.1-gd.lib
vtkImagingStatistics-8.1-gd.lib
vtkImagingStencil-8.1-gd.lib
vtkInfovisCore-8.1-gd.lib
vtkInfovisLayout-8.1-gd.lib
vtkInteractionImage-8.1-gd.lib
vtkInteractionStyle-8.1-gd.lib
vtkInteractionWidgets-8.1-gd.lib
vtkIOAMR-8.1-gd.lib
vtkIOCore-8.1-gd.lib
vtkIOEnSight-8.1-gd.lib
vtkIOExodus-8.1-gd.lib
vtkIOExport-8.1-gd.lib
vtkIOExportOpenGL-8.1-gd.lib
vtkIOExportOpenGL2-8.1-gd.lib
vtkIOGeometry-8.1-gd.lib
vtkIOImage-8.1-gd.lib
vtkIOImport-8.1-gd.lib
vtkIOInfovis-8.1-gd.lib
vtkIOLegacy-8.1-gd.lib
vtkIOLSDyna-8.1-gd.lib
vtkIOMINC-8.1-gd.lib
vtkIOMovie-8.1-gd.lib
vtkIONetCDF-8.1-gd.lib
vtkIOParallel-8.1-gd.lib
vtkIOParallelXML-8.1-gd.lib
vtkIOPLY-8.1-gd.lib
vtkIOSQL-8.1-gd.lib
vtkIOTecplotTable-8.1-gd.lib
vtkIOVideo-8.1-gd.lib
vtkIOXML-8.1-gd.lib
vtkIOXMLParser-8.1-gd.lib
vtkjpeg-8.1-gd.lib
vtkjsoncpp-8.1-gd.lib
vtklibharu-8.1-gd.lib
vtklibxml2-8.1-gd.lib
vtklz4-8.1-gd.lib
vtkmetaio-8.1-gd.lib
vtkNetCDF-8.1-gd.lib
vtknetcdfcpp-8.1-gd.lib
vtkoggtheora-8.1-gd.lib
vtkParallelCore-8.1-gd.lib
vtkpng-8.1-gd.lib
vtkproj4-8.1-gd.lib
vtkRenderingAnnotation-8.1-gd.lib
vtkRenderingContext2D-8.1-gd.lib
vtkRenderingContextOpenGL-8.1-gd.lib
vtkRenderingContextOpenGL2-8.1-gd.lib
vtkRenderingCore-8.1-gd.lib
vtkRenderingFreeType-8.1-gd.lib
vtkRenderingGL2PS-8.1-gd.lib
vtkRenderingGL2PSOpenGL2-8.1-gd.lib
vtkRenderingImage-8.1-gd.lib
vtkRenderingLabel-8.1-gd.lib
vtkRenderingLIC-8.1-gd.lib
vtkRenderingLOD-8.1-gd.lib
vtkRenderingOpenGL-8.1-gd.lib
vtkRenderingOpenGL2-8.1-gd.lib
vtkRenderingQt-8.1-gd.lib
vtkRenderingVolume-8.1-gd.lib
vtkRenderingVolumeOpenGL-8.1-gd.lib
vtkRenderingVolumeOpenGL2-8.1-gd.lib
vtksqlite-8.1-gd.lib
vtksys-8.1-gd.lib
vtkTestingGenericBridge-8.1-gd.lib
vtkTestingIOSQL-8.1-gd.lib
vtkTestingRendering-8.1-gd.lib
vtktiff-8.1-gd.lib
vtkverdict-8.1-gd.lib
vtkViewsContext2D-8.1-gd.lib
vtkViewsCore-8.1-gd.lib
vtkViewsInfovis-8.1-gd.lib
vtkViewsQt-8.1-gd.lib
vtkzlib-8.1-gd.lib
QVTKWidgetPlugin-gd.lib
```


至此，开发环境配置全部完成。

## PCL官方例程
PCL官方例程是在CMake下构建VS项目，然后用VS编译。现在我将官方给出的例程[PCL Visualizer in Qt with CMake](http://pointclouds.org/documentation/tutorials/qt_visualizer.php#qt-visualizer)直接用VS进行构建。程序效果如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418100643146.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9sb25nMzYx,size_16,color_FFFFFF,t_70)
项目完整源代码可前往GitHub下载，其中也包含了本人的项目属性表文件"**PropertySheet1.props**"：[https://github.com/xiaolong361/PCL_Visualizer](https://github.com/xiaolong361/PCL_Visualizer)
