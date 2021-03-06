CMAKE_MINIMUM_REQUIRED(VERSION 2.6)

if(EXISTS /lib64/security/)
    set(PAM_MODULES_LOCATION /lib64/security/)
elseif(EXISTS /lib/security/)
    set(PAM_MODULES_LOCATION /lib/security/)
elseif(EXISTS /lib/${CMAKE_ARCH_TRIPLET}/security)
    set(PAM_MODULES_LOCATION /lib/${CMAKE_ARCH_TRIPLET}/security/)
endif(EXISTS /lib64/security/)

MESSAGE("PAM_MODULES_LOCATION", ${PAM_MODULES_LOCATION})

PROJECT( PAM_FACE_AUTHENTICATION )
SET( QT_FACETRAINER_SRCS
     ${CMAKE_SOURCE_DIR}/src/main.cpp
     ${CMAKE_SOURCE_DIR}/src/faceTrainer.cpp
     ${CMAKE_SOURCE_DIR}/src/faceTrainerAdvSettings.cpp
     ${CMAKE_SOURCE_DIR}/src/opencvWebcam.cpp
     ${CMAKE_SOURCE_DIR}/src/detector.cpp
     ${CMAKE_SOURCE_DIR}/src/faceDetector.cpp
     ${CMAKE_SOURCE_DIR}/src/eyesDetector.cpp
     ${CMAKE_SOURCE_DIR}/src/tracker.cpp
     ${CMAKE_SOURCE_DIR}/src/verifier.cpp
     ${CMAKE_SOURCE_DIR}/src/utils.cpp
     ${CMAKE_SOURCE_DIR}/src/qtUtils.cpp
     ${CMAKE_SOURCE_DIR}/src/webcamImagePaint.cpp
   )

SET( QT_FACEVERIFYER_SRCS
     ${CMAKE_SOURCE_DIR}/src/fv_main.cpp
     ${CMAKE_SOURCE_DIR}/src/faceVerifier.cpp
     ${CMAKE_SOURCE_DIR}/src/opencvWebcam.cpp
     ${CMAKE_SOURCE_DIR}/src/detector.cpp
     ${CMAKE_SOURCE_DIR}/src/faceDetector.cpp
     ${CMAKE_SOURCE_DIR}/src/eyesDetector.cpp
     ${CMAKE_SOURCE_DIR}/src/tracker.cpp
     ${CMAKE_SOURCE_DIR}/src/verifier.cpp
     ${CMAKE_SOURCE_DIR}/src/utils.cpp
     ${CMAKE_SOURCE_DIR}/src/qtUtils.cpp
     ${CMAKE_SOURCE_DIR}/src/webcamImagePaint.cpp
   )

SET( QT_FACETRAINER_TRANS ${CMAKE_SOURCE_DIR}/trans/qt_facetrainer_pt_PT.ts
${CMAKE_SOURCE_DIR}/trans/qt_facetrainer_ru_RU.ts
${CMAKE_SOURCE_DIR}/trans/qt_facetrainer_de.ts
${CMAKE_SOURCE_DIR}/trans/qt_facetrainer_es.ts
${CMAKE_SOURCE_DIR}/trans/qt_facetrainer_fr.ts
${CMAKE_SOURCE_DIR}/trans/qt_facetrainer_it.ts
${CMAKE_SOURCE_DIR}/trans/qt_facetrainer_nl.ts
${CMAKE_SOURCE_DIR}/trans/qt_facetrainer_sv.ts
${CMAKE_SOURCE_DIR}/trans/qt_facetrainer_tr.ts
${CMAKE_SOURCE_DIR}/trans/qt_facetrainer_uk.ts


)

SET( QT_FACETRAINER_MOC_HDRS
     ${CMAKE_SOURCE_DIR}/include/faceTrainer.h
     ${CMAKE_SOURCE_DIR}/include/faceTrainerAdvSettings.h
     ${CMAKE_SOURCE_DIR}/include/aboutBox.h
   )

SET( QT_FACETRAINER_UIS
     ${CMAKE_SOURCE_DIR}/data/ui/uifiles/faceTrainer.ui
     ${CMAKE_SOURCE_DIR}/data/ui/uifiles/faceTrainerAdvSettings.ui
     ${CMAKE_SOURCE_DIR}/data/ui/uifiles/aboutDialog.ui

   )

SET( QT_FACETRAINER_RCS
     graphics.qrc
   )

SET( QT_FACEVERIFYER_MOC_HDRS
     ${CMAKE_SOURCE_DIR}/include/faceVerifier.h
     ${CMAKE_SOURCE_DIR}/include/aboutBox.h
   )

SET( QT_FACEVERIFYER_UIS
     ${CMAKE_SOURCE_DIR}/data/ui/uifiles/faceVerifier.ui
     ${CMAKE_SOURCE_DIR}/data/ui/uifiles/aboutDialog.ui

   )

SET( QT_FACEVERIFYER_RCS
     graphics.qrc
   )


SET( PAM_FACE_AUTHENTICATE_SRCS
     ${CMAKE_SOURCE_DIR}/src/pam_face_authentication.cpp
     ${CMAKE_SOURCE_DIR}/src/opencvWebcam.cpp
     ${CMAKE_SOURCE_DIR}/src/detector.cpp
     ${CMAKE_SOURCE_DIR}/src/faceDetector.cpp
     ${CMAKE_SOURCE_DIR}/src/eyesDetector.cpp
     ${CMAKE_SOURCE_DIR}/src/webcamImagePaint.cpp
     ${CMAKE_SOURCE_DIR}/src/tracker.cpp
     ${CMAKE_SOURCE_DIR}/src/verifier.cpp
     ${CMAKE_SOURCE_DIR}/src/utils.cpp
   )



SET( HAAR_CASCADES
     ${CMAKE_SOURCE_DIR}/data/haarcascades/haarcascade_eye.xml
     ${CMAKE_SOURCE_DIR}/data/haarcascades/haarcascade_eye_tree_eyeglasses.xml
     ${CMAKE_SOURCE_DIR}/data/haarcascades/haarcascade_nose.xml
     ${CMAKE_SOURCE_DIR}/data/haarcascades/haarcascade.xml
     ${CMAKE_SOURCE_DIR}/data/haarcascades/config.xml
   )

SET(BIN_INSTALL_DIR
    "/bin"
    CACHE PATH "The subdirectory relative to the install prefix for executables (default is bin)"
   )

SET(DATA_INSTALL_DIR
    "/share"
    CACHE PATH "The subdirectory relative to the install prefix where shared data will be installed (default is share)."
   )

FIND_PACKAGE( Gettext REQUIRED )
FIND_PACKAGE( X11 REQUIRED )
FIND_PACKAGE( Qt4 REQUIRED )
SET(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake/modules/")
FIND_PACKAGE(OpenCV REQUIRED)
SET( VERSION "0.4" )
SET( PKGDATADIR "${CMAKE_INSTALL_PREFIX}/${DATA_INSTALL_DIR}" )
SET( BINDIR "${CMAKE_INSTALL_PREFIX}/${BIN_INSTALL_DIR}" )
SET( XWINDOW_BINDIR "${CMAKE_INSTALL_PREFIX}/kde/4/${BIN_INSTALL_DIR}" )
ADD_DEFINITIONS (-DVERSION='"${VERSION}"' -DPKGDATADIR=\"${PKGDATADIR}\" -DBINDIR=\"${BINDIR}\")
FIND_PACKAGE( PAM REQUIRED )
FIND_PACKAGE( Gettext REQUIRED )
INCLUDE( ${QT_USE_FILE} )
QT4_ADD_TRANSLATION(QM ${QT_FACETRAINER_TRANS} )
QT4_ADD_RESOURCES( QT_FACETRAINER_RC_SRCS ${QT_FACETRAINER_RCS} )
QT4_WRAP_UI( QT_FACETRAINER_UI_HDRS ${QT_FACETRAINER_UIS} )
QT4_WRAP_CPP( QT_FACETRAINER_MOC_SRCS ${QT_FACETRAINER_MOC_HDRS} )
QT4_WRAP_UI( QT_FACEVERIFYER_UI_HDRS ${QT_FACEVERIFYER_UIS} )
QT4_WRAP_CPP( QT_FACEVERIFYER_MOC_SRCS ${QT_FACEVERIFYER_MOC_HDRS} )

IF( DEFINED PFA_GEN_STATS)
	ADD_DEFINITIONS (-DPFA_GEN_STATS=\"${PFA_GEN_STATS}\")
ENDIF( DEFINED PFA_GEN_STATS)

INCLUDE_DIRECTORIES( "${CMAKE_SOURCE_DIR}/include" )
INCLUDE_DIRECTORIES( ${CMAKE_BINARY_DIR} )
INCLUDE_DIRECTORIES(${OpenCV_INCLUDE_DIR})
INCLUDE_DIRECTORIES(${PAM_INCLUDE_DIR})
ADD_LIBRARY(pam_face_authentication SHARED ${PAM_FACE_AUTHENTICATE_SRCS})
SET_TARGET_PROPERTIES( pam_face_authentication PROPERTIES PREFIX "")

ADD_EXECUTABLE( qt-facetrainer ${QT_FACETRAINER_SRCS}
    ${QT_FACETRAINER_MOC_SRCS} ${QT_FACETRAINER_RC_SRCS}
    ${QT_FACETRAINER_UI_HDRS} ${QM} )
ADD_EXECUTABLE( qt-faceverifyer ${QT_FACEVERIFYER_SRCS}
    ${QT_FACEVERIFYER_MOC_SRCS} ${QT_FACETRAINER_RC_SRCS}
    ${QT_FACEVERIFYER_UI_HDRS} ${QM} )
TARGET_LINK_LIBRARIES( pam_face_authentication  ${PAM_LIBRARIES} ${OpenCV_LIBS})
TARGET_LINK_LIBRARIES( qt-facetrainer ${QT_LIBRARIES} ${OpenCV_LIBS})
TARGET_LINK_LIBRARIES( qt-faceverifyer ${QT_LIBRARIES} ${OpenCV_LIBS})
set(LANGUAGES pt_PT ru_RU es fr it nl sv tr de uk)

if( UNIX AND NOT APPLE )
    if( NOT DESKTOP_ENTRY )
      set( DESKTOP_ENTRY qt-facetrainer.desktop )
    endif( NOT DESKTOP_ENTRY )

    add_custom_command( OUTPUT ${DESKTOP_ENTRY}
      COMMAND touch ${DESKTOP_ENTRY}
      COMMAND sh ${CMAKE_SOURCE_DIR}/qt-facetrainer-desktop.sh ${CMAKE_INSTALL_PREFIX} >${DESKTOP_ENTRY}
      DEPENDS ${CMAKE_SOURCE_DIR}/qt-facetrainer-desktop.sh
      COMMENT "Generating desktop entry file"
      )
    add_custom_target( DESKTOP_ENTRY_FILE ALL
      DEPENDS ${DESKTOP_ENTRY}
      )
    set( APP_ICON ${CMAKE_SOURCE_DIR}/data/ui/images/pfa-logo.png )
    set_directory_properties(PROPERTIES ADDITIONAL_MAKE_CLEAN_FILES "${DESKTOP_ENTRY}" )
 foreach(LANG ${LANGUAGES})
    add_custom_command( OUTPUT LANG/${LANG}/LC_MESSAGES/pam_face_authentication.mo
    COMMAND mkdir -p LANG
    COMMAND mkdir -p LANG/${LANG}
    COMMAND mkdir -p LANG/${LANG}/LC_MESSAGES/
    COMMAND msgfmt -c -v -o LANG/${LANG}/LC_MESSAGES/pam_face_authentication.mo ${CMAKE_SOURCE_DIR}/trans/pam_face_authentication_${LANG}.po
    COMMENT "Generating translation files for pam_face_authentication")
   add_custom_target(LANG_${LANG} ALL DEPENDS
       LANG/${LANG}/LC_MESSAGES/pam_face_authentication.mo )
   install( FILES ${CMAKE_CURRENT_BINARY_DIR}/LANG/${LANG}/LC_MESSAGES/pam_face_authentication.mo DESTINATION
       ${PKGDATADIR}/locale/${LANG}/LC_MESSAGES/ )

     set_directory_properties(PROPERTIES ADDITIONAL_MAKE_CLEAN_FILES  "${CMAKE_CURRENT_BINARY_DIR}/LANG" )
endforeach(LANG)

  endif( UNIX AND NOT APPLE )
CONFIGURE_FILE(
      "${CMAKE_CURRENT_SOURCE_DIR}/cmake_uninstall.cmake.in"
        "${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake"
          IMMEDIATE @ONLY)

      ADD_CUSTOM_TARGET(uninstall
            "${CMAKE_COMMAND}" -P
            "${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake")


INSTALL(
    TARGETS
    pam_face_authentication
    DESTINATION
    ${PAM_MODULES_LOCATION}
)

INSTALL(
    TARGETS
    qt-faceverifyer
    DESTINATION
    ${BINDIR}
)

INSTALL(
    TARGETS
    qt-facetrainer
    DESTINATION
    ${BINDIR}
)

INSTALL(
    FILES
    ${HAAR_CASCADES}
    DESTINATION
    ${PKGDATADIR}
)

INSTALL(
    FILES
    ${APP_ICON}
    DESTINATION
    ${PKGDATADIR}/icons/
)

INSTALL( FILES ${CMAKE_CURRENT_BINARY_DIR}/${DESKTOP_ENTRY} DESTINATION ${PKGDATADIR}/applications )

