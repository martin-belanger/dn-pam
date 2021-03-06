#
# Copyright (c) 2005, 2006, 2009, 2011 Thorsten Kukuk <kukuk@suse.de>
#

CLEANFILES = *~
MAINTAINERCLEANFILES = $(MANS) README

EXTRA_DIST = README md5.c md5_crypt.c lckpwdf.-c $(MANS) CHANGELOG \
		tst-pam_hash-only $(XMLS)

man_MANS = pam_hash-only.8 unix_chkpwd.8 unix_update.8
XMLS = README.xml pam_hash-only.8.xml unix_chkpwd.8.xml unix_update.8.xml

TESTS = tst-pam_hash-only

securelibdir = $(SECUREDIR)
secureconfdir = $(SCONFIGDIR)

AM_CFLAGS = \
	-DCHKPWD_HELPER=\"$(sbindir)/unix_chkpwd\" \
	-DUPDATE_HELPER=\"$(sbindir)/unix_update\" \
	@TIRPC_CFLAGS@ @NSL_CFLAGS@

if HAVE_LIBSELINUX
  AM_CFLAGS += -D"WITH_SELINUX"
endif

pam_hash_only_la_LDFLAGS = -no-undefined -avoid-version -module
if HAVE_VERSIONING
  pam_hash_only_la_LDFLAGS += -Wl,--version-script=$(srcdir)/../modules.map
endif
pam_hash_only_la_LDFLAGS += -lpam

#pam_hash_only_la_LIBADD = $(top_builddir)/libpam/libpam.la \
#	@LIBCRYPT@ @LIBSELINUX@ @TIRPC_LIBS@ @NSL_LIBS@
pam_hash_only_la_LIBADD = \
	@LIBCRYPT@ @LIBSELINUX@ @TIRPC_LIBS@ @NSL_LIBS@

securelib_LTLIBRARIES = pam_hash-only.la

noinst_HEADERS = md5.h support.h yppasswd.h bigcrypt.h passverify.h

sbin_PROGRAMS = unix_chkpwd unix_update

noinst_PROGRAMS = bigcrypt

pam_hash_only_la_SOURCES = bigcrypt.c pam_acct.c \
	pam_auth.c pam_passwd.c pam_sess.c support.c \
	passverify.c yppasswd_xdr.c md5_good.c md5_broken.c

bigcrypt_SOURCES = bigcrypt.c bigcrypt_main.c
bigcrypt_CFLAGS = $(AM_CFLAGS)
bigcrypt_LDADD = @LIBCRYPT@

unix_chkpwd_SOURCES = unix_chkpwd.c md5_good.c md5_broken.c bigcrypt.c \
	passverify.c
unix_chkpwd_CFLAGS = $(AM_CFLAGS) @PIE_CFLAGS@ -DHELPER_COMPILE=\"unix_chkpwd\"
unix_chkpwd_LDFLAGS = @PIE_LDFLAGS@
unix_chkpwd_LDADD = @LIBCRYPT@ @LIBSELINUX@ @LIBAUDIT@

unix_update_SOURCES = unix_update.c md5_good.c md5_broken.c bigcrypt.c \
	passverify.c
unix_update_CFLAGS = $(AM_CFLAGS) @PIE_CFLAGS@ -DHELPER_COMPILE=\"unix_update\"
unix_update_LDFLAGS = @PIE_LDFLAGS@
unix_update_LDADD = @LIBCRYPT@ @LIBSELINUX@

if ENABLE_REGENERATE_MAN
noinst_DATA = README
README: pam_hash-only.8.xml
-include $(top_srcdir)/Make.xml.rules
endif
