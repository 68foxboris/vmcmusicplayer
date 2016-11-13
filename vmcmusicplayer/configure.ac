AC_INIT([vmcmusicplayer],[3.2.0],[enigma2-devel@lists.elitedvb.net])
AM_INIT_AUTOMAKE([dist-bzip2 foreign no-define tar-pax])
AC_CONFIG_HEADERS([enigma2-plugins-config.h])

# Silent rules are available since 1.11, but older versions
# are still in use. So don't use them unconditionally.
m4_ifdef([AM_SILENT_RULES], [AM_SILENT_RULES([yes])])

AC_USE_SYSTEM_EXTENSIONS
AC_SYS_LARGEFILE

AC_PROG_CC
AC_PROG_CXX
m4_ifdef([LT_INIT], [LT_INIT], [AC_PROG_LIBTOOL])
AC_CONFIG_MACRO_DIR([m4])

AM_PATH_PYTHON
AX_PYTHON_DEVEL
AX_PTHREAD

#dnl versions of gstreamer and plugins-base
AC_ARG_WITH(gstversion,
	AS_HELP_STRING([--with-gstversion],[use gstreamer version (major.minor)]),
	[GST_MAJORMINOR=$withval],[GST_MAJORMINOR=1.0])

PKG_CHECK_MODULES(ENIGMA2, enigma2)
PKG_CHECK_MODULES(GSTREAMER, gstreamer-$GST_MAJORMINOR gstreamer-pbutils-$GST_MAJORMINOR)
PKG_CHECK_MODULES(LIBCRYPTO, libcrypto)

AC_ARG_WITH(debug,
	AS_HELP_STRING([--without-debug],[disable debugging code]),
	[with_debug="$withval"],[with_debug="yes"])
if test "$with_debug" = "yes"; then
	DEBUG_CFLAGS="-ggdb3"
	AC_DEFINE([DEBUG],[1],[Define to 1 to enable debugging code])
fi
AC_SUBST(DEBUG_CFLAGS)

SAVE_CXXFLAGS="$CXXFLAGS"
CXXFLAGS="$PYTHON_CPPFLAGS -include Python.h $ENIGMA2_CFLAGS $CXXFLAGS"
SAVE_LDFLAGS="$LDFLAGS"
LDFLAGS="$PYTHON_LIBS $LDFLAGS -shared -fPIC"
 
AC_LANG(C++)
AC_MSG_CHECKING([for openpli iservice.h version(2)])
AC_LINK_IFELSE(
   [AC_LANG_PROGRAM(
[#include <enigma2/lib/service/iservice.h>
class MyPlayableService: public iPlayableService
{
   void AddRef(){}
   void Release(){}
 
     RESULT connectEvent(const Slot2<void,iPlayableService*,int> &event, ePtr<eConnection> &connection){return -1;}
     RESULT start(){return -1;}
     RESULT stop(){return -1;}
     RESULT setTarget(int target, bool noaudio=false){return -1;}; //only this function was changed
     SWIG_VOID(RESULT) seek(ePtr<iSeekableService> &SWIG_OUTPUT){};
     SWIG_VOID(RESULT) pause(ePtr<iPauseableService> &SWIG_OUTPUT){};
     SWIG_VOID(RESULT) info(ePtr<iServiceInformation> &SWIG_OUTPUT){};
     SWIG_VOID(RESULT) audioTracks(ePtr<iAudioTrackSelection> &SWIG_OUTPUT){};
     SWIG_VOID(RESULT) audioChannel(ePtr<iAudioChannelSelection> &SWIG_OUTPUT){};
     SWIG_VOID(RESULT) subServices(ePtr<iSubserviceList> &SWIG_OUTPUT){};
     SWIG_VOID(RESULT) frontendInfo(ePtr<iFrontendInformation> &SWIG_OUTPUT){};
     SWIG_VOID(RESULT) timeshift(ePtr<iTimeshiftService> &SWIG_OUTPUT){};
     SWIG_VOID(RESULT) cueSheet(ePtr<iCueSheet> &SWIG_OUTPUT){};
     SWIG_VOID(RESULT) subtitle(ePtr<iSubtitleOutput> &SWIG_OUTPUT){};
     SWIG_VOID(RESULT) audioDelay(ePtr<iAudioDelay> &SWIG_OUTPUT){};
     SWIG_VOID(RESULT) rdsDecoder(ePtr<iRdsDecoder> &SWIG_OUTPUT){};
     SWIG_VOID(RESULT) stream(ePtr<iStreamableService> &SWIG_OUTPUT){};
     SWIG_VOID(RESULT) streamed(ePtr<iStreamedService> &SWIG_OUTPUT){};
     SWIG_VOID(RESULT) keys(ePtr<iServiceKeys> &SWIG_OUTPUT){};
};],[MyPlayableService service;])
     ],[
        AC_MSG_RESULT([yes])
        AC_DEFINE(OPENPLI_ISERVICE_VERSION, 2)
     ],[
         AC_MSG_RESULT([no])
         AC_DEFINE(OPENPLI_ISERVICE_VERSION, 1)
     ])
 
CXXFLAGS="$SAVE_CXXFLAGS"
LDFLAGS="$SAVE_LDFLAGS"
 
 
AC_CONFIG_FILES([
Makefile

])
AC_OUTPUT
