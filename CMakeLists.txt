cmake_minimum_required(VERSION 3.14)

project(KDVStocks LANGUAGES CXX VERSION 1.0)

set(CMAKE_INCLUDE_CURRENT_DIR ON)

set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

SET( MAIN_APP_NAME "KDVStocks" )

# QtCreator supports the following variables for Android, which are identical to qmake Android variables.
# Check http://doc.qt.io/qt-5/deployment-android.html for more information.
# They need to be set before the find_package(Qt5 ...) call.

#if(ANDROID)
#    set(ANDROID_PACKAGE_SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/android")
#    if (ANDROID_ABI STREQUAL "armeabi-v7a")
#        set(ANDROID_EXTRA_LIBS
#            ${CMAKE_CURRENT_SOURCE_DIR}/path/to/libcrypto.so
#            ${CMAKE_CURRENT_SOURCE_DIR}/path/to/libssl.so)
#    endif()
#endif()

find_package(QT NAMES Qt6 Qt5 COMPONENTS Core Quick LinguistTools REQUIRED)
find_package(Qt${QT_VERSION_MAJOR} COMPONENTS Core Quick LinguistTools REQUIRED)

set(TS_FILES KDVStocks_en_US.ts)


add_executable(${MAIN_APP_NAME}
    main.cpp
    qml.qrc
    ${TS_FILES}
)

target_compile_definitions(${MAIN_APP_NAME}
  PRIVATE $<$<OR:$<CONFIG:Debug>,$<CONFIG:RelWithDebInfo>>:QT_QML_DEBUG>)

########################################################################
# Add KDVStocksCoreLib library
########################################################################
ADD_SUBDIRECTORY( KDVStocksCoreLib )

########################################################################
# Include headers
########################################################################
target_include_directories( ${MAIN_APP_NAME} PRIVATE ${KDV_STOCKS_CORE_LIB_PUBLIC_INCLUDE_DIRS} )

########################################################################
# Link libraries
########################################################################
target_link_libraries( ${MAIN_APP_NAME}
  PRIVATE Qt${QT_VERSION_MAJOR}::Core Qt${QT_VERSION_MAJOR}::Quick ${KDV_STOCKS_CORE_LIB_NAME})

########################################################################
# Translations
########################################################################
qt5_create_translation(QM_FILES ${CMAKE_SOURCE_DIR} ${TS_FILES})


