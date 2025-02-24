cmake_minimum_required(VERSION 3.5)

project(svgeditor VERSION 0.1 LANGUAGES CXX)

set(CMAKE_INCLUDE_CURRENT_DIR ON)

set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(QT NAMES Qt6 Qt5 COMPONENTS Widgets REQUIRED)
find_package(Qt${QT_VERSION_MAJOR} COMPONENTS Widgets REQUIRED)

set(RES_SOURCES
	res/icons.qrc
	res/qss.qrc
)

set(INCLUDE_SOURCES
	include/mainwindow.h
	include/svgcanvasview.h
	include/utility.h
	include/lcanvasitem.h
	include/lcanvasshape.h
	include/lcanvasline.h
	include/lcanvasrect.h
	include/lcanvasellipse.h
	include/lcanvaspolygon.h
	include/lcanvastext.h
	include/lcanvasview.h
	include/lcanvasscene.h
)

set(SRC_SOURCES
	src/main.cpp
	src/mainwindow.cpp
	src/svgcanvasview.cpp
	src/utility.cpp
	src/lcanvasitem.cpp
	src/lcanvasshape.cpp
	src/lcanvasline.cpp
	src/lcanvasrect.cpp
	src/lcanvasellipse.cpp
	src/lcanvaspolygon.cpp
	src/lcanvastext.cpp
	src/lcanvasview.cpp
	src/lcanvasscene.cpp
)

set(PROJECT_SOURCES
	${RES_SOURCES}
	${INCLUDE_SOURCES}
	${SRC_SOURCES}
)

if(${QT_VERSION_MAJOR} GREATER_EQUAL 6)
	qt_add_executable(svgeditor
        MANUAL_FINALIZATION
        ${PROJECT_SOURCES}
    )
# Define target properties for Android with Qt 6 as:
#    set_property(TARGET svgeditor APPEND PROPERTY QT_ANDROID_PACKAGE_SOURCE_DIR
#                 ${CMAKE_CURRENT_SOURCE_DIR}/android)
# For more information, see https://doc.qt.io/qt-6/qt-add-executable.html#target-creation
else()
    if(ANDROID)
		add_library(svgeditor SHARED
            ${PROJECT_SOURCES}
        )
# Define properties for Android with Qt 5 after find_package() calls as:
#    set(ANDROID_PACKAGE_SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/android")
    else()
		add_executable(svgeditor
            ${PROJECT_SOURCES}
        )
    endif()
endif()

target_include_directories(svgeditor
	PRIVATE
	${PROJECT_SOURCE_DIR}/include
)

target_link_libraries(svgeditor PRIVATE Qt${QT_VERSION_MAJOR}::Widgets)

set_target_properties(svgeditor PROPERTIES
    MACOSX_BUNDLE_GUI_IDENTIFIER my.example.com
    MACOSX_BUNDLE_BUNDLE_VERSION ${PROJECT_VERSION}
    MACOSX_BUNDLE_SHORT_VERSION_STRING ${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}
)

if(QT_VERSION_MAJOR EQUAL 6)
	qt_finalize_executable(svgeditor)
endif()
