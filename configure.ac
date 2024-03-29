#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

AC_PREREQ(2.64)

AC_INIT([apertium-tlh], [0.0.1], [https://github.com/apertium/apertium-tlh/issues],, [https://github.com/flammie/apertium-tlh/])
AM_INIT_AUTOMAKE([1.12 -Wall -Wno-portability -Werror foreign dist-xz no-define])
AC_PROG_LN_S
AC_PROG_AWK

m4_define([required_apertium_version], [3.3.0])
PKG_CHECK_MODULES(APERTIUM,   apertium >= required_apertium_version)


PKG_CHECK_MODULES(HFST, hfst >= 3.4.0)

AC_ARG_ENABLE([lexc],
              [AS_HELP_STRING([--enable-lexc],
                              [use HFST to compile XEROX versions of stuffs @<:@default=yes@:>@])],
              [enable_lexc=$enableval], [enable_lexc=yes])
AM_CONDITIONAL([WANT_LEXC], [test x$enable_lexc != xno])
AC_ARG_ENABLE([ltproc],
              [AS_HELP_STRING([--enable-ltproc],
                              [use apertium to compile real apertium version @<:@default=no@:>@])],
              [enable_ltproc=$enableval], [enable_ltproc=no])
AM_CONDITIONAL([WANT_LTPROC], [test x$enable_ltproc != xno])


AC_PATH_PROG([LTPRINT], [lt-print], [no])

AS_IF([test x$ac_cv_path_LTPRINT = x],
      [AC_MSG_ERROR([You do not have lt-print installed.])])
AS_IF([test x$ac_cv_path_LTPRINT = xno,],
      [AC_MSG_ERROR([You do not have lt-print installed.])])

AC_PATH_PROG(CGCOMP, cg-comp, no)
AS_IF([test x$ac_cv_path_CGCOMP = xno],
      [AC_MSG_ERROR([You do not have cg-comp installed.])])
AS_IF([test x$ac_cv_path_CGPROC = xno],
      [AC_MSG_ERROR([You do not have cg-comp installed.])])

AC_PATH_PROG(PKG_CONFIG, pkg-config, no)
AS_IF([test x$ac_cv_path_PKG_CONFIG = x],
      [AC_MSG_ERROR([You do not have pkg-config installed.])])
AS_IF([test x$ac_cv_path_PKG_CONFIG = xno],
      [AC_MSG_ERROR([You do not have pkg-config installed.])])


PKG_CHECK_MODULES(REGTEST, apertium-regtest >= 0.0.1, [],
                  [AC_MSG_WARN([Running tests requires apertium-regtest])])

AP_MKINCLUDE

AC_CONFIG_FILES([Makefile apertium-tlh.pc])
AC_OUTPUT

cat <<EOF
Configured $PACKAGE_STRING
* using hfst-lexc: $enable_lexc
* using ltproc: $enable_ltproc
EOF

