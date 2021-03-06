# configure.ac for GPG4Win
# Copyright (C) 2005, 2006, 2007, 2008, 2010 g10 Code GmbH
#
# This file is part of GPG4Win
#
# GPG4Win is free software; you can redistribute it and/or modify it
# under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 2 of the License, or
# (at your option) any later version.
#
# GPG4Win is distributed in the hope that it will be useful, but
# WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU
# General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; if not, see <http://www.gnu.org/licenses/>.

# (Process this file with autoconf to produce a configure script.)

AC_PREREQ(2.59)
min_automake_version="1.9.3"

# To build a release you need to create a tag with the version number
# (git tag -s gpg4win-2.x.y) and run "./autogen.sh --force".  Please
# bump the version number immediately after the release and do another
# commit and push so that the git magic is able to work.
m4_define([my_version], [3.0.3])

# Below is m4 magic to extract and compute the git revision number,
# the decimalized short revision number, a beta version string and a
# flag indicating a development version (my_isgit). The latter works
# by requiring the final tag in the repository.
m4_define([git_revision],
          m4_esyscmd([git rev-parse --short HEAD | tr -d '\n\r']))
m4_define([git_revision_dec],
          m4_esyscmd_s([echo $((0x$(echo ]git_revision[|head -c 4)))]))
m4_define([git_betastring],
          m4_esyscmd_s([git describe --tags --match 'gpg4win-[0-9].*.*[0-9]' \
                       --long | awk -F- '$3!=0{print"-beta"$3}']))
m4_define([my_isgit],m4_if(git_betastring,[],[no],[yes]))
m4_define([my_full_version],[my_version[]git_betastring])

AC_INIT([gpg4win],[my_full_version],[gpg4win-devel@wald.intevation.org])

# Remember to update these version numbers also in the NEWS file.
MANUALVERSION_EINSTEIGER="2.0.2"
MANUALDATE_EINSTEIGER="30. Mai 2006"
MANUALVERSION_DURCHBLICKER="2.0.2"
MANUALDATE_DURCHBLICKER="30. Mai 2006"
MANUALVERSION_NOVICES="1.0.0"
MANUALDATE_NOVICES="Nov. 30, 2006"
MANUALVERSION_ADVANCED="0.0.0"
MANUALDATE_ADVANCED="Nov. 30, 2006"
COMPENDIUM_VERSION_DE="4.0.0"
COMPENDIUM_DATE_DE="23. Juni 2017"
COMPENDIUMINPROGRESS_DE="zuletzt geringf�gig korrigiert am 10. Januar 2018"
COMPENDIUM_VERSION_EN="3.0.0"
COMPENDIUM_DATE_EN="30th November 2016"
COMPENDIUMINPROGRESS_EN="last minor changes from 30th November 2016"

MANUALINPROGRESS_DE=m4_if(my_isgit,[yes],["~und evtl. seitdem weiter bearbeitet"])
MANUALINPROGRESS_EN=m4_if(my_isgit,[yes],["~and may have been further edited"])
PACKAGE=$PACKAGE_NAME
VERSION=$PACKAGE_VERSION
GIT_REVISION=git_revision
GIT_REVISION_DEC=git_revision_dec


AC_CONFIG_SRCDIR(src/config.nsi.in)
AC_CONFIG_MACRO_DIR(m4)

AM_INIT_AUTOMAKE([tar-ustar])
AM_MAINTAINER_MODE
AC_CANONICAL_HOST
AM_CONFIG_HEADER(config.h)

# Language support
AM_PO_SUBDIRS


AC_SUBST(PACKAGE)
AC_SUBST(VERSION)
AC_SUBST(MANUALVERSION_EINSTEIGER)
AC_SUBST(MANUALDATE_EINSTEIGER)
AC_SUBST(MANUALVERSION_DURCHBLICKER)
AC_SUBST(MANUALDATE_DURCHBLICKER)
AC_SUBST(MANUALINPROGRESS_DE)
AC_SUBST(MANUALVERSION_NOVICES)
AC_SUBST(MANUALDATE_NOVICES)
AC_SUBST(MANUALVERSION_ADVANCED)
AC_SUBST(MANUALDATE_ADVANCED)
AC_SUBST(MANUALINPROGRESS_EN)
AC_SUBST(COMPENDIUM_VERSION_DE)
AC_SUBST(COMPENDIUM_DATE_DE)
AC_SUBST(COMPENDIUMINPROGRESS_DE)
AC_SUBST(COMPENDIUM_VERSION_EN)
AC_SUBST(COMPENDIUM_DATE_EN)
AC_SUBST(COMPENDIUMINPROGRESS_EN)

if echo "${PACKAGE_VERSION}" | egrep 'svn|rc|beta|git' >/dev/null ; then
  IS_BETA_BUILD=yes
else
  IS_BETA_BUILD=no
fi
AC_SUBST(IS_BETA_BUILD)

# Generate extended version information.  Note, that for NSIS use we
# must not translate the dots to commas in the BUILD_FILEVERSION.
BUILD_TIMESTAMP=`date --iso-8601=minutes`
BUILD_ISODATE=`date --iso-8601`
changequote(,)dnl
BUILD_FILEVERSION=`echo "$VERSION" | sed 's/\([0-9.]*\).*/\1./'`
changequote([,])dnl
BUILD_FILEVERSION="${BUILD_FILEVERSION}${GIT_REVISION_DEC}"
AC_SUBST(BUILD_TIMESTAMP)
AC_SUBST(BUILD_ISODATE)
AC_SUBST(BUILD_FILEVERSION)

# To test the manuals using an actual web server, the preview target
# may be used in doc/manuals.  The default is my (wk) test environment
# but may be overriden using another preview variable.  PREVIEWHOST is
# expected to be the target for an rsync command.
if test -z "$PREVIEWHOST" ; then
   PREVIEWHOST="vigenere:/var/www/all/preview.gpg4win.org/htdocs/"
fi
AC_SUBST(PREVIEWHOST)

# The final release is copied to an archive server.  The default
# location may be changed by setting the RELEASEHOST envvar.
if test -z "$RELEASEHOST" ; then
   RELEASEHOST="vigenere:tarballs/gpg4win"
   if test "${IS_BETA_BUILD}" = yes ; then
      RELEASEHOST="${RELEASEHOST}/Beta"
   fi
fi
AC_SUBST(RELEASEHOST)

# Substitutions to set generated files in a Emacs buffer to read-only.
AC_SUBST(emacs_local_vars_begin, ['Local Variables:'])
AC_SUBST(emacs_local_vars_read_only, ['buffer-read-only: t'])
AC_SUBST(emacs_local_vars_end, ['End:'])

# For development of this package it is useful to build without
# creating the source installer (takes a long time).  This option may
# be used to suppress creation of the source installer.
AC_ARG_ENABLE(source-installer,
    AC_HELP_STRING([--disable-source-installer],
                   [Do not create the Source installer]),
                    build_source_installer=$enableval,
                    build_source_installer=yes)
AM_CONDITIONAL(BUILD_SOURCE_INSTALLER, test "$build_source_installer" = yes)


# You can build a light version of the installer in addition to the
# regular version.  This one does not contain the manuals.
AC_ARG_ENABLE(light-installer,
    AC_HELP_STRING([--disable-light-installer],
                   [Do not create the light version of the installer]),
                    build_light_installer=$enableval,
                    build_light_installer=no)
AM_CONDITIONAL(BUILD_LIGHT_INSTALLER, test "$build_light_installer" = yes)

# You can build a vanilla version of the installer, featuring only
# GnuPG, a pinentry and GPGME, in addition to the regular version.
# This one does not contain the manuals.
AC_ARG_ENABLE(vanilla-installer,
    AC_HELP_STRING([--disable-vanilla-installer],
                   [Do not create the vanilla version of the installer]),
                    build_vanilla_installer=$enableval,
                    build_vanilla_installer=no)
AM_CONDITIONAL(BUILD_VANILLA_INSTALLER, test "$build_vanilla_installer" = yes)

# If you want to build only the light or vanilla installer, you may
# disable building of the full installer.
AC_ARG_ENABLE(full-installer,
    AC_HELP_STRING([--disable-full-installer],
                   [Do not create the full version of the installer]),
                    build_full_installer=$enableval,
                    build_full_installer=yes)
AM_CONDITIONAL(BUILD_FULL_INSTALLER, test "$build_full_installer" = yes)

# For development it is often faster to build without manuals.
AC_ARG_ENABLE(manuals,
    AC_HELP_STRING([--disable-manuals],
                   [Do not build the manuals (for debugging)]),
                    build_manuals=$enableval,
                    build_manuals=yes)
AM_CONDITIONAL(BUILD_MANUALS, test "$build_manuals" = yes)

# Additionaly enable building gpgex for another host
AC_ARG_WITH([additional-gpgex-host],
    AC_HELP_STRING([--with-additional-gpgex-host=HOST],
                   [Additionaly build gpgex for this host]),
                   [if test "$withval" = yes; then
                      AC_MSG_FAILURE(
                          [--with-additional-gpgex-host was given,
                           but no host specified])
                    else
                      gpgex_host=$withval
                    fi
                   ],
                    gpgex_host=no)

if test "$gpgex_host" != no; then
    GPGEX_ADD_HOST="$gpgex_host"
fi
AC_SUBST(GPGEX_ADD_HOST)

AC_ARG_ENABLE(fast-makensis,
    AC_HELP_STRING([--enable-fast-makensis],
                   [Use a less efficient compressor to speed up building]),
                    use_fast_makensis=$enableval,
                    use_fast_makensis=no)
if test "$use_fast_makensis" = yes; then
  EXTRA_MAKENSIS_FLAGS="${EXTRA_MAKENSIS_FLAGS} -DDISABLE_LZMA=1"
fi
AC_SUBST(EXTRA_MAKENSIS_FLAGS)


# Required tools.
AC_PROG_CC
AC_PROG_CPP
AC_PROG_RANLIB
AC_CHECK_TOOL(AR, ar, :)
AC_CHECK_TOOLS(STRIP, strip)
AC_CHECK_TOOLS(DLLTOOL, dlltool)
AC_CHECK_PROGS(MAKE, make)
AC_CHECK_PROGS(UNZIP, unzip)
AC_CHECK_PROGS(TAR, tar)
AC_CHECK_PROGS(MKDIR, mkdir)
AC_CHECK_PROGS(CP, cp)
AC_CHECK_PROGS(RM, rm)
AC_CHECK_PROGS(STOW, stow)
AC_CHECK_PROGS(MAKENSIS, makensis)
AC_CHECK_PROGS(ZCAT, zcat)
AC_CHECK_PROGS(TEXI2DVI, texi2dvi)
AC_CHECK_PROGS(DVIPDF, dvipdf)
AC_CHECK_PROGS(CONVERT, convert)
AC_CHECK_PROGS(SHA1SUM, sha1sum)
AC_CHECK_PROGS(MSGFMT, msgfmt)
AC_CHECK_PROGS(GITLOG_TO_CHANGELOG, gitlog-to-changelog,
               [build-aux/gitlog-to-changelog])
AC_CHECK_PROGS(BUILD_CC, gcc cc)
AC_PROG_INSTALL

if test -n "$GPGEX_ADD_HOST"; then
    AC_CHECK_TOOLS(STRIP_EX, ${GPGEX_ADD_HOST}-strip)
fi

if test -z "$GPG4WIN_PARALLEL"; then
    JOBS=$(nproc 2>/dev/null || echo 1)
    GPG4WIN_PARALLEL=-j$JOBS
    AC_MSG_NOTICE([Using autodetected $JOBS make jobs. You can override this by setting GPG4WIN_PARALLEL.])
fi
AC_SUBST(GPG4WIN_PARALLEL)

missing_tools=
for i in DLLTOOL MAKE UNZIP TAR MKDIR CP RM STOW MAKENSIS ZCAT TEXI2DVI \
         DVIPDF CONVERT SHA1SUM MSGFMT BUILD_CC; do
   eval tmp='$'$i
   if test -z "$tmp"; then
      missing_tools="$missing_tools `echo $i | tr 'A-Z' 'a-z'`"
   fi
done

# Additional runtime libraries from mingw.
#GPG4WIN_RUNTIME_LIBRARY(libgcc_s_sjlj-1)
#GPG4WIN_RUNTIME_LIBRARY(libstdc++-6)
GPG4WIN_RUNTIME_LIBRARY(libwinpthread-1)

AC_CHECK_PROGS(GLIB_GENMARSHAL, glib-genmarshal)
AC_CHECK_PROGS(GLIB_COMPILE_SCHEMAS, glib-compile-schemas)
AC_CHECK_PROGS(GLIB_COMPILE_RESOURCES, glib-compile-resources)
AC_CHECK_PROGS(GDK_PIXBUF_CSOURCE, gdk-pixbuf-csource)


missing_opt_tools=
for i in GLIB_GENMARSHAL GLIB_COMPILE_SCHEMAS GLIB_COMPILE_RESOURCES \
         GDK_PIXBUF_CSOURCE; do
   eval tmp='$'$i
   if test -z "$tmp"; then
      missing_opt_tools="$missing_opt_tools `echo $i | tr 'A-Z' 'a-z'`"
   fi
done



# Optional tools.

AC_CHECK_PROGS(WGET, wget)


# Packages.
gpg4win_dup_sources=""


GPG4WIN_SPKG([zlib])

GPG4WIN_BPKG_GTK([pkgconfig])

GPG4WIN_SPKG([libffi], [pkgconfig])
GPG4WIN_SPKG([glib], [libiconv gettext pkgconfig libffi zlib])
# GPG4WIN_BPKG_GTK_DEV([atk], [glib pkgconfig])
# GPG4WIN_BPKG_GTK_DEV([pango], [glib pkgconfig])
# GPG4WIN_BPKG_GTK_DEV([cairo], [glib pkgconfig])
GPG4WIN_BPKG_GTK_DEV([expat])
# GPG4WIN_BPKG_GTK_DEV([freetype], [pkgconfig zlib])
# GPG4WIN_BPKG_GTK_DEV([fontconfig], [pkgconfig zlib expat])
# GPG4WIN_SPKG([gdk-pixbuf], [glib])
# GPG4WIN_SPKG([gtk+], [libiconv gettext pkgconfig atk pango dnl
#                       glib gdk-pixbuf cairo])
GPG4WIN_SPKG([libpng], [pkgconfig])

GPG4WIN_SPKG([bzip2])
GPG4WIN_SPKG([libgpg-error], [libiconv gettext])
GPG4WIN_SPKG([libassuan], [libgpg-error w32pth])
# GPG4WIN_SPKG([pinentry], [qtbase libiconv gtk+])
GPG4WIN_SPKG([gpgme], [libgpg-error glib qtbase])
# GPG4WIN_SPKG([gpgol], [gpgme libassuan])
# GPG4WIN_SPKG([gpgex], [libassuan])
# GPG4WIN_SPKG([paperkey])
# GPG4WIN_SPKG([gpa], [libiconv gettext zlib gtk+ libpng glib gpgme])
GPG4WIN_SPKG([w32pth])
GPG4WIN_SPKG([libiconv])
GPG4WIN_SPKG([gettext], [libiconv])


# Packages needed for gpgex
GPG4WIN_SPKGEX([libgpg-error])
GPG4WIN_SPKGEX([libassuan], [libgpg-error])
# GPG4WIN_SPKGEX([gpgex], [libassuan])
GPG4WIN_SPKGEX([gpgme], [libgpg-error libassuan])
# GPG4WIN_SPKGEX([gpgol], [gpgme])

# GPG4WIN_IPKG([compendium])

# The GnuPG Package.
GPG4WIN_BPKG_BINSRC([gnupg-w32])
# See the script kde-l10n.sh how this package is created.
# GPG4WIN_BPKG_BINSRC([kde-l10n])

# Qt related stuff.
GPG4WIN_SPKG([qtbase], [libpng])
GPG4WIN_SPKG([qttools], [qtbase])
GPG4WIN_SPKG([qttranslations], [qtsvg qttools qtwinextras])
GPG4WIN_SPKG([qtwinextras], [qtbase])
GPG4WIN_SPKG([qtsvg], [qtbase])
GPG4WIN_SPKG([boost], [expat zlib bzip2])
# GPG4WIN_KDEPKG([breeze-icons], [extra-cmake-modules qtbase])
# GPG4WIN_KDEPKG([kleopatra], [gnupg-w32 breeze-icons kxmlgui libkleo kde-l10n kitemmodels qttranslations])
# GPG4WIN_KDEPKG([extra-cmake-modules])
# GPG4WIN_KDEPKG([kconfig], [qtbase qttools extra-cmake-modules])
# GPG4WIN_KDEPKG([ki18n], [qtbase gettext qttools extra-cmake-modules])
# GPG4WIN_KDEPKG([kwidgetsaddons], [kconfig qttools qtbase])
# GPG4WIN_KDEPKG([kcompletion], [kwidgetsaddons])
# GPG4WIN_KDEPKG([kwindowsystem], [qtbase qtwinextras])
# GPG4WIN_KDEPKG([kcoreaddons], [qtbase qttools])
# GPG4WIN_KDEPKG([kcodecs], [qtbase qttools])
# GPG4WIN_KDEPKG([kguiaddons], [qtbase qttools])
# GPG4WIN_KDEPKG([kmime], [kcodecs])
# GPG4WIN_KDEPKG([kconfigwidgets], [kwidgetsaddons kguiaddons ki18n kcodecs kconfig])
# GPG4WIN_KDEPKG([kitemviews], [qtbase qttools])
# GPG4WIN_KDEPKG([kitemmodels], [qtbase qttools])
# GPG4WIN_KDEPKG([karchive], [qtbase qttools extra-cmake-modules])
# GPG4WIN_KDEPKG([kiconthemes], [qtsvg kconfigwidgets karchive])
# GPG4WIN_KDEPKG([kxmlgui], [kiconthemes kconfigwidgets kitemviews])
# GPG4WIN_KDEPKG([libkleo], [kmime kwidgetsaddons kwindowsystem kcompletion kconfig gpgme])

GPG4WIN_FINALIZE

# Throw an error if required tools are missing
if test -n "$missing_tools"; then
    for i in $missing_tools; do
      AC_MSG_NOTICE([$i is missing])
    done
    AC_MSG_ERROR([[
***
*** Required tools not found. Please consult the above messages
*** and install them before running configure again.
***]])
fi

# Throw an error if any duplicate source packages have been found
if test -n "$gpg4win_dup_sources"; then
    tmp=
    for i in $gpg4win_dup_sources; do
      tmp="$tmp
$i"
    done
    tmp2="
You might remove them with:"
    for i in $gpg4win_rm_candidates; do
        if ! grep -q $(basename $i) packages/packages.current; then
            tmp2="$tmp2
rm $i"
        fi
    done
    AC_MSG_ERROR([Packages with more that one source version:$tmp$tmp2])
fi


# Finalize.

AC_CONFIG_FILES(Makefile)
AC_CONFIG_FILES(packages/Makefile src/Makefile po/Makefile.in)
AC_CONFIG_FILES(po/build-cc)
AC_CONFIG_FILES(src/config.nsi src/gpg4win.mk)
AC_CONFIG_FILES(doc/Makefile)
AC_CONFIG_FILES(doc/logo/Makefile)
AC_CONFIG_FILES(doc/manual/Makefile doc/manual/version.tex)
AC_CONFIG_FILES(src/toolchain.cmake)
AC_OUTPUT

# Throw a warning if optional tools are missing
if test -n "$missing_opt_tools"; then
    AC_MSG_WARN([[
***
*** Some tools not found.  They may or may not be needed depending on
*** the packages you want to include.  For example GLIB and GTK+
*** require certain tools; you may need to install a recent version of
*** GLIB and GTK+ on the build machine to allow for cross-compiling.
***]])
    for i in $missing_opt_tools; do
      AC_MSG_NOTICE([$i is missing])
    done
fi

echo -n "
        $PACKAGE_NAME-$PACKAGE_VERSION prepared for make

        Revision: ${GIT_REVISION}  (${GIT_REVISION_DEC})
        32bit platform: $host
        64bit platform: ${GPGEX_ADD_HOST}
"
if test -n "$JOBS"; then
    echo "        make jobs: ${JOBS}"
else
    echo ""
fi
