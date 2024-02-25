# Mediapipe for unreal

This repository is based on Wongfei's work: https://github.com/wongfei/ue4-mediapipe-plugin. The original author stopped updating the project 2 years ago. So, the project can't be directly built because of some dependencies' changes during this time. To solve this, I found some tricks and edited some scripts. Please give me a star if helpful to you.


Platforms: UE 4.27 / 5.0 (Windows ONLY, other will not be supported)

2D features: Face, Iris, Hands, Pose, Holistic

3D features: Face Mesh, World Pose

Demo video: https://www.youtube.com/watch?v=_gRGjGn6FQE

## (Optional) Building the plugin and MediaPipe wrapper library

### Clone plugin

`git clone https://github.com/Raysquare/Mediapipe-for-Unreal.git`

### Clone wrapper

`git clone https://github.com/Raysquare/mediapipe.git ue4-mediapipe-wrapper`

### Setup Mediapipe 
1. Install MSYS2 and edit the %PATH% environment variable. If MSYS2 is installed to C:\msys64, add C:\msys64\usr\bin to your %PATH% environment variable. https://www.msys2.org/
2. Install necessary packages.
`C:\> pacman -S git patch unzip
3. Install Python and allow the executable to edit the %PATH% environment variable. Download Python Windows executable from https://www.python.org/downloads and install.
4. Install Visual C++ Build Tools 2019 and WinSDK. **Can't use VS2022!! Please download the correct version.** https://visualstudio.microsoft.com/zh-hans/vs/older-downloads/
5. Install Bazel or Bazelisk and add the location of the Bazel executable to the %PATH% environment variable. **Newest version of bazel doesn't work. I only found that version 3.72 is working**. https://github.com/bazelbuild/bazel/releases?q=3.72&expanded=true
6. Set Bazel variables. 
Please find the exact paths and version numbers from your local version. Tips: You can found your MSVC version at %PathForVSBuildTools%/VC/Tools/MSVC. And your win10 SDK in visual studio installer.
```
C:\> set BAZEL_VS=C:\Program Files (x86)\Microsoft Visual Studio\2019\BuildTools
C:\> set BAZEL_VC=C:\Program Files (x86)\Microsoft Visual Studio\2019\BuildTools\VC
C:\> set BAZEL_VC_FULL_VERSION=<Your local VC version>
C:\> set BAZEL_WINSDK_FULL_VERSION=<Your local WinSDK version>
```
7. Install OpenCV 3.4.10. Recommand install path: C:\opencv :https://github.com/opencv/opencv/releases/tag/3.4.10

If you found any troubles, you also can learn from https://developers.google.com/mediapipe/framework/getting_started/install#installing_on_windows. But do not try to install the latest version of those dependencies since the mediapipe in this repo is not latest.

### Build wrapper

`cd ue4-mediapipe-wrapper`

`bazel build -c opt --define MEDIAPIPE_DISABLE_GPU=1 --action_env PYTHON_BIN_PATH="C:\\Python39\\python.exe" mediapipe/unreal:ump_shared`

See: `ue4-mediapipe-wrapper\mediapipe\unreal\scripts\build_shared.cmd`

Copy ump_shared.dll to `ue4-mediapipe-plugin\Plugins\MediaPipe\ThirdParty\mediapipe\Binaries\Win64\`

See: `ue4-mediapipe-wrapper\mediapipe\unreal\scripts\deploy.cmd`

### Other deps

Protobuf 3.11.4

Copy libprotobuf.lib to `ue4-mediapipe-plugin\Plugins\MediaPipe\ThirdParty\protobuf\Lib\Win64\`

(Optional) OpenCV 3.4.10 with fixed CAP_MSMF

`git clone -b msmf_fix_3410 https://github.com/wongfei/opencv.git`

Copy opencv_world3410.dll to `ue4-mediapipe-plugin\Plugins\MediaPipe\ThirdParty\mediapipe\Binaries\Win64\`
