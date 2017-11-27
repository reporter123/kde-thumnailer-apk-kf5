project(kde-thumbnailer-apk)
cmake_minimum_required(VERSION 2.8.12)

set(SHARED_MIME_INFO_MINIMUM_VERSION "0.30")
find_package(ECM 0.0.11 REQUIRED NO_MODULE)
set(CMAKE_MODULE_PATH ${ECM_MODULE_PATH} ${ECM_KDE_MODULE_DIR})

include(KDEInstallDirs)
include(KDECMakeSettings)
include(KDECompilerSettings)
include(FeatureSummary)

find_package(Qt5 REQUIRED COMPONENTS Widgets)
find_package(KF5 REQUIRED COMPONENTS KDELibs4Support) #CoreAddons Solid 

find_package(SharedMimeInfo REQUIRED)

add_definitions(${QT_DEFINITIONS}) # ${KDE4_DEFINITIONS}
include_directories(${QT_INCLUDES}) # ${KDE4_INCLUDES}

set(apkthumbnail_SRCS apkcreator.cpp)

kde4_add_plugin(apkthumbnail ${apkthumbnail_SRCS})
target_link_libraries(apkthumbnail
    KF5::KDELibs4Support KF5::KIOCore KF5::Archive
)

install(TARGETS apkthumbnail DESTINATION ${PLUGIN_INSTALL_DIR})
install(FILES apkthumbnail.desktop DESTINATION ${SERVICES_INSTALL_DIR})
install(FILES apk.xml DESTINATION ${XDG_MIME_INSTALL_DIR})
update_xdg_mimetypes(${XDG_MIME_INSTALL_DIR})