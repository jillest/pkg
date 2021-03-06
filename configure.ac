m4_define([maj_ver], [1])
m4_define([med_ver], [3])
m4_define([min_ver], [0])
m4_define([api_ver], [m4_eval(maj_ver * 1000000 + med_ver * 1000 + min_ver)])
m4_define([pkg_version], [maj_ver.med_ver.min_ver.b2])

AC_INIT([pkg],[pkg_version],[https://github.com/freebsd/pkg],[pkg])
AC_CONFIG_SRCDIR([configure.ac])
AM_INIT_AUTOMAKE([1.11 foreign -Wno-portability no-dist-gzip dist-xz])
AM_SILENT_RULES([yes])

# Here are rules for setting version-info for libpkg
# we use 3 integers: current, revision and age:
# - if you change ABI (meaning add/remove/modify), then always update `current`
# - if you modify lib (that means ALL modifies), then increase `revision`
# - if you want backward compatibility, then use `age`
#
# more details here: https://www.flameeyes.eu/autotools-mythbuster/libtool/version.html
#
# For git you should consider using of the proper branch model, when a set of
# related changes that leads to `revision` increase should always be in a separate
# branch. For details you can check this resource:
# http://nvie.com/posts/a-successful-git-branching-model/
LIBPKG_CURRENT=3
LIBPKG_REVISION=0
LIBPKG_AGE=0
LIBPKG_SO_VERSION="$LIBPKG_CURRENT:$LIBPKG_REVISION:$LIBPKG_AGE"
AC_SUBST(LIBPKG_SO_VERSION)

AC_PROG_CC_C99
LT_INIT()
AC_CONFIG_MACRO_DIR([m4])
AX_CFLAGS_WARN_ALL

AC_C_CONST
AC_TYPE_SIZE_T
AC_CHECK_FUNCS(memmove bcopy)
AC_PROG_LN_S

AC_CHECK_HEADERS_ONCE([fcntl.h unistd.h elf-hints.h])
AC_TYPE_OFF_T
AC_FUNC_MMAP

if test "$ac_cv_func_mmap_fixed_mapped" = "yes"; then
    FILEMAP=unixfilemap
else
    FILEMAP=readfilemap
fi
AC_SUBST(FILEMAP)

AC_MSG_CHECKING(for GCC atomic builtins)
AC_LINK_IFELSE([
	AC_LANG_SOURCE([[
		int main() {
			volatile unsigned long val = 1;
			__sync_synchronize();
			__sync_val_compare_and_swap(&val, 1, 0);
			__sync_add_and_fetch(&val, 1);
			__sync_sub_and_fetch(&val, 1);
			return 0;
		}
	]])
],
[
 	AC_MSG_RESULT([yes])
	AC_DEFINE([HAVE_ATOMIC_BUILTINS], [1], [Has gcc/MSVC atomic intrinsics])
],
[
 	AC_MSG_RESULT([no])
	AC_DEFINE([HAVE_ATOMIC_BUILTINS], [0], [Has gcc/MSVC atomic intrinsics])
])

AC_CHECK_HEADERS_ONCE([machine/endian.h])
AC_CHECK_HEADERS_ONCE([endian.h])
AC_CHECK_HEADERS_ONCE([sys/endian.h])
AC_CHECK_HEADERS_ONCE([fcntl.h])
AC_CHECK_HEADERS_ONCE([dlfcn.h])
AC_CHECK_HEADERS_ONCE([memory.h])
AC_CHECK_HEADERS_ONCE([sys/types.h])
AC_CHECK_HEADERS_ONCE([sys/stat.h])
AC_CHECK_HEADERS_ONCE([stdlib.h])
AC_CHECK_HEADERS_ONCE([string.h])
AC_CHECK_HEADERS_ONCE([memory.h])
AC_CHECK_HEADERS_ONCE([inttypes.h])
AC_CHECK_HEADERS_ONCE([unistd.h])
AC_CHECK_HEADERS_ONCE([dlfcn.h])
AC_CHECK_HEADERS_ONCE([fcntl.h])

AC_CHECK_HEADERS_ONCE([sys/types.h])
AC_CHECK_HEADERS_ONCE([sys/stat.h])
AC_CHECK_HEADERS_ONCE([sys/param.h])
AC_CHECK_HEADERS_ONCE([sys/mman.h])
AC_CHECK_HEADERS_ONCE([stdlib.h])
AC_CHECK_HEADERS_ONCE([stddef.h])
AC_CHECK_HEADERS_ONCE([stdarg.h])
AC_CHECK_HEADERS_ONCE([stdbool.h])
AC_CHECK_HEADERS_ONCE([stdint.h])
AC_CHECK_HEADERS_ONCE([string.h])
AC_CHECK_HEADERS_ONCE([unistd.h])
AC_CHECK_HEADERS_ONCE([ctype.h])
AC_CHECK_HEADERS_ONCE([errno.h])
AC_CHECK_HEADERS_ONCE([limits.h])
AC_CHECK_HEADERS_ONCE([libgen.h])
AC_CHECK_HEADERS_ONCE([stdio.h])
AC_CHECK_HEADERS_ONCE([float.h])
AC_CHECK_HEADERS_ONCE([math.h])

AC_CHECK_HEADER([regex.h], [
	AC_DEFINE(HAVE_REGEX_H, 1, [Define to 1 if you have the <regex.h> header file.])
	],
	[AC_MSG_ERROR([unable to find the regex.h header])],
	[#include <sys/types.h>])

AC_CHECK_FUNCS_ONCE([posix_fallocate])
AC_CHECK_FUNCS_ONCE([usleep])
AC_CHECK_FUNCS_ONCE([localtime_r])
AC_CHECK_FUNCS_ONCE([gmtime_r])
AC_CHECK_FUNCS_ONCE([strerror_r])

AC_DEFUN([AC_PROG_GIT], [AC_CHECK_PROG(GITBIN,git,yes)])
AC_PROG_GIT
if test x"${GITBIN}" == x"yes" ; then
	AC_MSG_CHECKING([for git head])
	if test -d "${GIT_DIR:-${ac_top_srcdir:-./}/.git}" ; then
		GIT_HEAD=`git rev-parse --short HEAD`
		if test -n "`git diff-index -m --name-only HEAD`" ; then
			GIT_HEAD=${GIT_HEAD}-dirty
		fi
		if test -n "${GIT_HEAD}" ; then
			GIT_HEAD="-${GIT_HEAD}"
		fi
	fi
fi
AH_TEMPLATE(GIT_HEAD, "Git revision")
AH_TEMPLATE(PKG_API, "Api version")
AC_MSG_RESULT([$GIT_HEAD])
AC_SUBST([GIT_HEAD])
AC_DEFINE_UNQUOTED([GIT_HEAD], $GIT_HEAD)
AC_DEFINE([PKG_API], [api_ver])


AC_SEARCH_LIBS([lzma_version_string], [lzma], [], [
  AC_MSG_ERROR([unable to find the liblzma])
])
AC_SEARCH_LIBS([BZ2_bzReadOpen], [bz2], [], [
  AC_MSG_ERROR([unable to find the libbz2])
])
AC_SEARCH_LIBS([zlibVersion], [z], [], [
  AC_MSG_ERROR([unable to find the zlib])
])
AC_SEARCH_LIBS([archive_read_open], [archive], [], [
  AC_MSG_ERROR([unable to find the archive_read() function])
])

AC_CHECK_HEADER([archive.h],
	[pkg_found_archive_headers=yes])
AS_IF([test "x$pkg_found_archive_headers" != "xyes"],
	[AC_MSG_ERROR([Unable to find the libarchive headers])])

AC_SEARCH_LIBS([jail_getid], [jail], [
	AC_DEFINE(HAVE_LIBJAIL, 1, [Define to 1 if you have the 'jail' library (-ljail).])
	LIBJAIL_LIB="-ljail"
	], [])

AC_CHECK_HEADER([gelf.h], [
	AC_DEFINE(HAVE_GELF_H, 1, [Define to 1 if you have the <gelf.h> header file.])
	AC_CHECK_LIB(elf, gelf_getehdr, [
		AC_DEFINE(HAVE_LIBELF, 1, [Define to 1 if you have the 'elf' library (-lelf).])
		LIBELF_LIB="-lelf"
	])
], [
	LIBELF_LIB="\$(top_builddir)/external/libelf.la"
	LIBELF_BUNDLED="libelf.la"
	LIBELF_INCLUDE="-I\$(top_srcdir)/external/libelf"
])

AC_CHECK_HEADER([sys/sbuf.h], [
	AC_DEFINE(HAVE_SYS_SBUF_H, 1, [Define to 1 if you have the <sys/sbuf.h> header file.])
	AC_CHECK_LIB(sbuf, sbuf_new, [
		AC_DEFINE(HAVE_LIBSBUF, 1, [Define to 1 if you have the 'sbuf' library (-lsbuf).])
		LIBSBUF_LIB="-lsbuf"
	])
], [
	LIBSBUF_LIB="\$(top_builddir)/external/libsbuf.la"
	LIBSBUF_BUNDLED="libsbuf.la"
	LIBSBUF_INCLUDE="-I\$(top_srcdir)/external/libsbuf"
])

AC_CHECK_HEADER([execinfo.h], [
	AC_DEFINE(HAVE_EXECINFO_H, 1, [Define to 1 if you have the <execinfo.h> header file.])
	AC_CHECK_LIB(execinfo, backtrace, [
		AC_DEFINE(HAVE_LIBEXECINFO, 1, [Define to 1 if you have the 'execinfo' library (-lexecinfo).])
		LIBEXECINFO_LIB="-lexecinfo"
	])
])

AC_ARG_WITH([ldns], AS_HELP_STRING([--with-ldns], [Build with ldns for name resolving]))

AS_IF([test "x$with_ldns" = "xyes"], [
   PKG_CHECK_MODULES([LDNS], [libldns], [AC_DEFINE([HAVE_LDNS], [1], [Use ldns])],
   [
   	AC_CHECK_HEADER([ldns/ldns.h], [
   		AC_CHECK_LIB(ldns, ldns_resolver_new_frm_file, [
   			AC_DEFINE([HAVE_LDNS], [1], [Use ldns])
   			AC_DEFINE([LDNS_LIBS], [-lldns], [LDNS library])
   		])
   	])
   ])
])

AC_CHECK_HEADER([atf-c.h], [
	 TESTS="\$(test_program)"
 ])

AC_CHECK_HEADER([sys/capability.h], [
        AC_CHECK_LIB(c, cap_sandboxed, [
                AC_DEFINE(HAVE_CAPSICUM, 1, [Define 1 if you have 'capsicum'.])
        ])

])

AC_SUBST([LIBELF_LIB])
AC_SUBST([LIBELF_BUNDLED])
AC_SUBST([LIBELF_INCLUDE])
AC_SUBST([LIBSBUF_LIB])
AC_SUBST([LIBSBUF_BUNDLED])
AC_SUBST([LIBSBUF_INCLUDE])
AC_SUBST([LIBJAIL_LIB])
AC_SUBST([LIBEXECINFO_LIB])
AC_SUBST([TESTS])
AC_SUBST([LDNS_LIBS])
AC_SUBST([LDNS_CFLAGS])

AC_CONFIG_FILES(Makefile \
		src/Makefile \
		external/Makefile \
		scripts/Makefile \
		docs/Makefile \
		tests/Makefile \
		libpkg/Makefile
		libpkg/pkg.h \
		libpkg/pkg.pc
		scripts/periodic/400.status-pkg
		scripts/periodic/410.pkg-audit
		scripts/periodic/411.pkg-backup
		scripts/periodic/460.pkg-checksum
		scripts/periodic/490.status-pkg-changes
		scripts/sbin/pkg2ng
		scripts/completion/_pkg.bash
		scripts/completion/_pkg.zsh)
AC_CONFIG_HEADERS(pkg_config.h)

AC_OUTPUT
