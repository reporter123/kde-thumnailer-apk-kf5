project(kde-thumbnailer-apk)
cmake_minimum_required(VERSION 3.0.2)
set(QT_MIN_VERSION "5.4.0")#Default logging levels implemented in 5.4+
set(KF5_MIN_VERSION "5.14.0")#5.14 needed for ecm_qt_declare_logging_category

set(SHARED_MIME_INFO_MINIMUM_VERSION "0.30")
find_package(ECM ${KF5_MIN_VERSION} REQUIRED NO_MODULE)
set(CMAKE_MODULE_PATH ${ECM_MODULE_PATH} ${ECM_KDE_MODULE_DIR})

include(KDEInstallDirs)
include(KDECMakeSettings)
include(KDECompilerSettings)

include(ECMQtDeclareLoggingCategory)
include(FeatureSummary)

find_package(Qt5 ${QT_MIN_VERSION} REQUIRED COMPONENTS Widgets Gui)
find_package(KF5 ${KF5_MIN_VERSION} REQUIRED COMPONENTS KIO Archive) #CoreAddons Solid 

find_package(SharedMimeInfo REQUIRED)

add_definitions(${QT_DEFINITIONS}) # ${KDE4_DEFINITIONS}
include_directories(${QT_INCLUDES}) # ${KDE4_INCLUDES}

set(apkthumbnail_SRCS apkcreator.cpp)

add_library(apkthumbnail MODULE ${apkthumbnail_SRCS})
target_link_libraries(apkthumbnail
    Qt5::Gui KF5::KIOCore KF5::Archive KF5::KIOWidgets
)

ecm_qt_declare_logging_category(apkthumbnail_SRCS 
            HEADER apkthumbnaildebug.h
            IDENTIFIER APKTHUMBNAIL
            CATEGORY_NAME kf5.kio.apkthumbnail)

install(TARGETS apkthumbnail DESTINATION ${PLUGIN_INSTALL_DIR})
install(FILES apkthumbnail.desktop DESTINATION ${SERVICES_INSTALL_DIR})
install(FILES apk.xml DESTINATION ${XDG_MIME_INSTALL_DIR})
update_xdg_mimetypes(${XDG_MIME_INSTALL_DIR})

feature_summary(WHAT ALL FATAL_ON_MISSING_REQUIRED_PACKAGES)
