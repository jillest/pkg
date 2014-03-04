libpkg_la_SOURCES=	pkg.c \
			backup.c \
			dns_utils.c \
			elfhints.c \
			fetch.c \
			packing.c \
			pkg_add.c \
			pkg_attributes.c \
			pkg_config.c \
			pkg_conflicts.c \
			pkg_cudf.c \
			pkg_create.c \
			pkg_delete.c \
			pkg_elf.c \
			pkg_event.c \
			pkg_jobs.c \
			pkg_manifest.c \
			pkg_ports.c \
			pkg_printf.c \
			pkg_repo.c \
			pkg_solve.c \
			pkg_status.c \
			pkg_version.c \
			pkgdb.c \
			pkgdb_repo.c \
			rcscripts.c \
			rsa.c \
			ssh.c \
			scripts.c \
			update.c \
			utils.c \
			plugins.c \
			pkg_old.c
libpkg_la_CFLAGS=	-I$(top_srcdir)/libpkg \
			@LIBELF_INCLUDE@ \
			-I$(top_srcdir)/external/libucl/include \
			-I$(top_srcdir)/external/uthash \
			-I$(top_srcdir)/external/sqlite \
			-I$(top_srcdir)/external/libyaml/include \
			-DPREFIX=\"$(prefix)\" \
			-Wno-pointer-sign
libpkg_la_LIBADD=	$(top_builddir)/external/libucl.la \
			$(top_builddir)/external/libsqlite.la \
			$(top_builddir)/external/libyaml.la \
			@LIBELF_LIB@ \
			-larchive \
			-lsbuf \
			-lfetch \
			-lutil \
			-lssl \
			-lcrypto
libpkg_la_LDFLAGS=	-version-info 1 -pthread
lib_LTLIBRARIES=	libpkg.la
include_HEADERS=	pkg.h

noinst_HEADERS=		private/db_upgrades.h \
			private/elf_tables.h \
			private/event.h \
			private/ldconfig.h \
			private/pkg.h \
			private/pkg_printf.h \
			private/pkgdb.h \
			private/repodb.h \
			private/thd_repo.h \
			private/utils.h

dist_man_MANS=		pkg-repository.5 \
			pkg_printf.3 \
			pkg_repos.3 