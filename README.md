# Performance
https://github.com/didi/booster
# APK SIZE
https://github.com/facebook/redex
https://tech.meituan.com/2017/04/07/android-shrink-overall-solution.html

# AndroidImprove

gradle assembleDebug --stacktrace
gradlew compileDebug --stacktrace 
compile DebugJavaWithJavac

adb logcat *:E

NestedScrollView fillport must with wrap_content child
https://stackoverflow.com/questions/37205997/nestedscrollview-could-not-scroll-with-match-parent-height-child

控制台乱码问题
https://blog.csdn.net/jankingmeaning/article/details/104772104?depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-6&utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-6

# monkey

adb shell monkey -p cn.com.bjx.bjxtalents --pct-syskeys 0  --pct-majornav 0 --pct-anyevent 0 --pct-nav 0 --pct-touch 100  -v 10000

# monkeyrunner
setx -m ANDROID_SWT "C:\Users\pang\AppData\Local\Android\Sdk\tools\lib\x86_64"

@echo off
rem Copyright (C) 2010 The Android Open Source Project
rem
rem Licensed under the Apache License, Version 2.0 (the "License");
rem you may not use this file except in compliance with the License.
rem You may obtain a copy of the License at
rem
rem      http://www.apache.org/licenses/LICENSE-2.0
rem
rem Unless required by applicable law or agreed to in writing, software
rem distributed under the License is distributed on an "AS IS" BASIS,
rem WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
rem See the License for the specific language governing permissions and
rem limitations under the License.

rem don't modify the caller's environment
setlocal

rem Set up prog to be the path of this script, including following symlinks,
rem and set up progdir to be the fully-qualified pathname of its directory.
set prog=%~f0

rem Change current directory and drive to where the script is, to avoid
rem issues with directories containing whitespaces.
cd /d %~dp0

rem Check we have a valid Java.exe in the path.
set java_exe=
call ..\lib\find_java.bat
if not defined java_exe goto :EOF
for /f %%a in ("%APP_HOME%\lib\monkeyrunner-25.3.2.jar") do set jarfile=%%~nxa
set frameworkdir=.
set libdir=

if exist %frameworkdir%\%jarfile% goto JarFileOk
    set frameworkdir=..\lib

if exist %frameworkdir%\%jarfile% goto JarFileOk
    set frameworkdir=..\framework

:JarFileOk

set jarpath=%frameworkdir%\%jarfile%

if not defined ANDROID_SWT goto QueryArch
    set swt_path=%ANDROID_SWT%
    goto SwtDone

:QueryArch

    for /f "delims=" %%a in ('%frameworkdir%\..\bin\archquery') do set swt_path=%frameworkdir%\%%a

:SwtDone

if exist "%swt_path%" goto SetPath
    echo SWT folder '%swt_path%' does not exist.
    echo Please set ANDROID_SWT to point to the folder containing swt.jar for your platform.
    exit /B

:SetPath

call "%java_exe%" -Xmx512m "-Djava.ext.dirs=%frameworkdir%;%swt_path%" -Dcom.android.monkeyrunner.bindir=..\..\platform-tools -jar %jarpath% %*


