# Visual Leak Detector (Support Visual Studio 2022)

## Introduction

Visual C++ provides built-in memory leak detection, but its capabilities are minimal at best. This memory leak detector was created as a free alternative to the built-in memory leak detector provided with Visual C++. Here are some of Visual Leak Detector's features, none of which exist in the built-in detector:

*   Provides a complete stack trace for each leaked block, including source file and line number information when available.
*   Detects most, if not all, types of in-process memory leaks including COM-based leaks, and pure Win32 heap-based leaks.
*   Selected modules (DLLs or even the main EXE) can be excluded from leak detection.
*   Provides complete data dumps (in hex and ASCII) of leaked blocks.
*   Customizable memory leak report: can be saved to a file or sent to the debugger and can include a variable level of detail.

Other after-market leak detectors for Visual C++ are already available. But most of the really popular ones, like Purify and BoundsChecker, are very expensive. A few free alternatives exist, but they're often too intrusive, restrictive, or unreliable. Visual Leak Detector is currently the only freely available memory leak detector for Visual C++ that provides all of the above professional-level features packaged neatly in an easy-to-use library.

## Contributing

We encourage developers who've added their own features, or fixed bugs they've found, to contribute to the project. The full version-controlled source tree is available publicly via Git at the URL below. Feel free to clone from this URL and submit patches for consideration for inclusion in future versions. You can also issue pull requests for changes that you've made and would like to share.

* Based on [Source code](https://github.com/oneiric/vld)
* [Source code](https://github.com/onbings/vld)

## Use the prebuilt binaries

You can find in the package directory  a release prebuilt version for Win32 and Win64 Os.
These one can be used easily with Cmake (see below)

## Build from the sources

Open the solution file 
* vld_vs2022 Visual studio 2022 (OnBings) 
* vld_vs2022 Visual studio 2019 (https://github.com/oneiric/vld)
and build the solution
The result files will be located in the src/bin subdirectory

REM: You need to have the MFC lib installed to be able to build the project



## Usage with CMAKE (OnBings)
\#\#\# VLD \#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#
add_library(vld SHARED IMPORTED) \# or STATIC instead of SHARED
set_target_properties(vld PROPERTIES
  IMPORTED_LOCATION "C:/pro/github/vld/package/Win64/vld_x64.dll"		\#Win32/vld_x86.dll for 32 bits
  IMPORTED_IMPLIB	"C:/pro/github/vld/package/Win64/vld.lib"
  INTERFACE_INCLUDE_DIRECTORIES "C:/pro/github/vld/package/include"
)
\# Error wil be displayed in visual studio immediate windows (use ctrl-alt-i to make it appears) and in c:\tm\vld.txt (see default vld.ini)
get_target_property(DllPath vld IMPORTED_LOCATION)
get_filename_component(DllDir ${DllPath} DIRECTORY)

add_custom_command(TARGET bofstd-tests POST_BUILD    
    \#COMMAND ${CMAKE_COMMAND} -E cmake_echo_color --cyan \"=DllPath================>\"
    COMMAND ${CMAKE_COMMAND} -E copy_if_different ${DllPath}  $<TARGET_FILE_DIR:bofstd-tests>
    COMMAND ${CMAKE_COMMAND} -E copy_if_different ${DllDir}/dbghelp.dll $<TARGET_FILE_DIR:bofstd-tests>
	COMMAND ${CMAKE_COMMAND} -E copy_if_different ${DllDir}/Microsoft.DTfW.DHL.manifest $<TARGET_FILE_DIR:bofstd-tests>
	COMMAND ${CMAKE_COMMAND} -E copy_if_different ${DllDir}/../vld.ini $<TARGET_FILE_DIR:bofstd-tests>
)   
\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#

target_link_libraries(bofstd-tests
  PRIVATE
    ONBINGS::bofstd-objects
    GTest::GTest
--->	$<$<AND:$<BOOL:${WIN32}>,$<CONFIG:Debug>>:vld>
)
Copyright © 2005-2021 VLD Team

 [1]: https://github.com/oneiric/vld/blob/master/COPYING.txt
 [2]: https://github.com/onbings/vld/COPYING.txt