#
# Copyright (c) 2005, 2006, 2007 Thorsten Kukuk <kukuk@thkukuk.de>
#

AUTOMAKE_OPTIONS = 1.9 gnu dist-bzip2 check-news

#SUBDIRS = tests modules po conf doc examples xtests
SUBDIRS = modules

CLEANFILES = *~

EXTRA_DIST = pgp.keys.asc Copyright Make.xml.rules

ACLOCAL_AMFLAGS = -I m4

release: dist releasedocs

release-docs: releasedocs

releasedocs:
	rm -rf dn-pam-$(VERSION)
	mkdir -p dn-pam-$(VERSION)/doc
	make -C doc releasedocs
	tar zfc dn-pam-$(VERSION)-docs.tar.gz \
		dn-pam-$(VERSION)/doc
	tar jfc dn-pam-$(VERSION)-docs.tar.bz2 \
		dn-pam-$(VERSION)/doc
	rm -rf dn-pam-$(VERSION)

xtests:
	make -C xtests xtests

.PHONY: xtests

gen_changelog_start_date = 2016-12-15
gen-ChangeLog:
	if test -d .git; then						\
		( $(top_srcdir)/gitlog-to-changelog --append-dot	\
		  --since=$(gen_changelog_start_date) &&		\
		  echo && echo &&					\
		) > $(distdir)/ChangeLog.new &&				\
		rm -f $(distdir)/ChangeLog &&				\
		mv $(distdir)/ChangeLog.new $(distdir)/ChangeLog;	\
	fi

dist-hook: gen-ChangeLog
.PHONY: gen-ChangeLog

export TAR_OPTIONS = --owner=0 --group=0 --numeric-owner --mode=go-w,go+rX
