project(kde-thumbnailer-apk)
cmake_minimum_required(VERSION 2.8.12)
set(QT_MIN_VERSION "5.2.0")

set(SHARED_MIME_INFO_MINIMUM_VERSION "0.30")
find_package(ECM 0.0.11 REQUIRED NO_MODULE)
set(CMAKE_MODULE_PATH ${ECM_MODULE_PATH} ${ECM_KDE_MODULE_DIR})

include(KDEInstallDirs)
include(KDECMakeSettings)
include(KDECompilerSettings)
include(FeatureSummary)

find_package(Qt5 ${QT_MIN_VERSION} REQUIRED COMPONENTS Widgets Gui)
find_package(KF5 REQUIRED COMPONENTS KIO Archive) #CoreAddons Solid 

find_package(SharedMimeInfo REQUIRED)

add_definitions(${QT_DEFINITIONS}) # ${KDE4_DEFINITIONS}
include_directories(${QT_INCLUDES}) # ${KDE4_INCLUDES}

set(apkthumbnail_SRCS apkcreator.cpp)

add_library(apkthumbnail MODULE ${apkthumbnail_SRCS})
target_link_libraries(apkthumbnail
    Qt5::Gui KF5::KIOCore KF5::Archive KF5::KIOWidgets
)

install(TARGETS apkthumbnail DESTINATION ${PLUGIN_INSTALL_DIR})
install(FILES apkthumbnail.desktop DESTINATION ${SERVICES_INSTALL_DIR})
install(FILES apk.xml DESTINATION ${XDG_MIME_INSTALL_DIR})
update_xdg_mimetypes(${XDG_MIME_INSTALL_DIR})
