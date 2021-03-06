# ------------------------------------------------------------
# GNU shogi and xshogi configuration script.
# ------------------------------------------------------------

dnl Process this file with autoconf to produce a configure script.

AC_PREREQ(2.57)
AC_INIT([gnushogi],[1.4.2+],[https://savannah.gnu.org/bugs/?group=gnushogi])
AM_INIT_AUTOMAKE
AC_CONFIG_SRCDIR([gnushogi/gnushogi.h])
AC_CONFIG_HEADERS(config.h)
AM_MAINTAINER_MODE

##########
AC_MSG_NOTICE([C compiler])
AC_PROG_CC
AC_CANONICAL_BUILD
AC_PROG_INSTALL
AM_PROG_CC_C_O

##########
AC_MSG_NOTICE([libs])

# curses
AC_ARG_WITH([curses],
  [AS_HELP_STRING([--with-curses],
    [enable curses UI (default: yes if available)])],
  [],
  [with_curses=check])

LIBCURSES=
AS_IF([test "x$with_curses" != xno],
  [AC_CHECK_LIB([curses], [clrtoeol],
    [AC_SUBST([LIBCURSES], [-lcurses])
     AC_SUBST([CURSESDSP], [gnushogi-cursesdsp.o])
     AC_SUBST([CURSESDSPMINI], [gnuminishogi-cursesdsp.o])
     AC_DEFINE([HAVE_LIBCURSES], [1],
               [Define if you have lib])
    ],
    [AS_IF([test "x$with_curses" = xyes],
      [AC_MSG_ERROR(
	 [--with-curses was given, but test for curses failed])])],
    [-ltermcap])])

# other libraries.
AC_CHECK_LIB([m], [pow])
AC_CHECK_LIB([termcap], [tgoto])



##########
# C compiler warnings.

if [[ $ac_cv_c_compiler_gnu = yes ]]
then
WARNINGS="-Wall -Wno-implicit-int -Wstrict-prototypes"
CEXTRAFLAGS="-fsigned-char -funroll-loops"
else
# Who knows what warnings your compiler uses?
WARNINGS=
CEXTRAFLAGS=
fi

AC_SUBST(WARNINGS)
AC_SUBST(CEXTRAFLAGS)

##########
AC_MSG_NOTICE([header files])

AC_HEADER_STDC
AC_HEADER_SYS_WAIT
AC_CHECK_HEADERS([fcntl.h stdlib.h string.h sys/file.h sys/ioctl.h])
AC_CHECK_HEADERS([sys/param.h sys/time.h unistd.h])
AC_CHECK_HEADERS(errno.h)
AC_HEADER_TIME

##########
AC_MSG_NOTICE([typedefs])

AC_C_CONST
AC_HEADER_STDBOOL
AC_C_INLINE
AC_TYPE_PID_T
AC_TYPE_SIZE_T
AC_HEADER_TIME
AC_STRUCT_TM

##########
AC_MSG_NOTICE([compiler characteristics])

AC_CHECK_SIZEOF(long)

##########
AC_MSG_NOTICE([library functions])

AC_PROG_GCC_TRADITIONAL
AC_TYPE_SIGNAL
AC_CHECK_FUNCS([gettimeofday memset pow])
AC_CHECK_FUNCS([strchr strerror strrchr strstr strtol])
AC_CHECK_FUNCS([memcpy bcopy])
AC_CHECK_FUNCS([setvbuf setlinebuf])

##########

# don't build pat2inc when cross-compiling, we don't need it
if test "$cross_compiling" = no; then
   AC_SUBST([PAT2INC], [pat2inc])
   AC_SUBST([PAT2INCEXE], [pat2inc$EXEEXT])
fi

##########
AC_CONFIG_FILES([Makefile
                 gnushogi/Makefile
                 doc/Makefile])
AC_OUTPUT
