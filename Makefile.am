
ACLOCAL_AMFLAGS = -I m4

SUBDIRS = src po doc lib

EXTRA_DIST = INSTALL COPYING AUTHORS NEWS README README.md README.md.in THANKS TODO ChangeLog

dist_noinst_SCRIPTS = gennews.sh autogen.sh configure.sh rebuild.sh dist.sh

DISTCLEANFILES = ChangeLog AUTHORS NEWS README README.md README.html README.txt

dist-hook:
	if test -x "`which git 2>/dev/null`" -a -d "$(srcdir)/.git" ; then \
		chmod u+w $(distdir)/{ChangeLog,AUTHORS,NEWS} ; \
		( cd $(srcdir) ; \
		  echo -e "# created with git log --stat=76 -M -C -300|fmt -sct -w80\n" ; \
		  git log --stat=76 -M -C -300|fmt -sct -w80 \
		)|head -5000>$(distdir)/ChangeLog ; \
		( cd $(srcdir) ; \
		  echo "$(PACKAGE) -- authors file.  $(DATE)" ; \
		  echo "" ; \
		  git log|grep '^Author:'|awk '{if(!authors[$$0]){print$$0;authors[$$0]=1;}}' \
		)>$(distdir)/AUTHORS ; \
		( cd $(srcdir) ; \
		  echo "$(PACKAGE) -- history of user-visible changes.  $(DATE)" ; \
		  PACKAGE="$(PACKAGE)" $(srcdir)/gennews.sh \
		)>$(distdir)/NEWS ; \
	fi ; \
	chmod u+w $(distdir)/{README.md,README} ; \
	sed -r -e 's:\@PACKAGE\@:$(PACKAGE):g;s:\@VERSION\@:$(VERSION):g;s:\@DATE\@:$(DATE):g' $(distdir)/README.md.in >$(distdir)/README.md ; \
	cat $(distdir)/README.md | fmt -sct -w80 -g72 >$(distdir)/README

ChangeLog:
	if test -x "`which git 2>/dev/null`" -a -d "$(srcdir)/.git" ; then \
		( cd $(srcdir) ; \
		  echo -e "# created with git log --stat=76 -M -C -300|fmt -sct -w80\n" ; \
		  git log --stat=76 -M -C -300|fmt -sct -w80 \
		)|head -5000>$@ ; \
	else touch $@ ; fi

AUTHORS:
	if test -x "`which git 2>/dev/null`" -a -d "$(srcdir)/.git" ; then \
		( cd $(srcdir) ; \
		  echo "$(PACKAGE) -- authors file.  $(DATE)" ; \
		  echo "" ; \
		  git log|grep '^Author:'|awk '{if(!authors[$$0]){print$$0;authors[$$0]=1;}}' \
		)>$@ ; \
	else touch $@ ; fi

NEWS:
	if test -x "`which git 2>/dev/null`" -a -d "$(srcdir)/.git" ; then \
		( cd $(srcdir) ; \
		  echo "$(PACKAGE) -- history of user-visible changes.  $(DATE)" ; \
		  PACKAGE="$(PACKAGE)" $(srcdir)/gennews.sh \
		)>$@ ; \
	else touch $@ ; fi

README.md: README.md.in
	sed -r -e 's:\@PACKAGE\@:$(PACKAGE):g;s:\@VERSION\@:$(VERSION):g;s:\@DATE\@:$(DATE):g' $< >$@

README: README.md
	cat $< | fmt -sct -w80 -g72 >$@

README.html: README.md
	$(MARKDOWN) $< >$@

README.txt: README.html
	links -dump -width 80 -codepage utf-8 $< >$@

INSTALL.cmakebuild: INSTALL-cmakebuild.md
	cat $< | fmt -sct -w80 -g72 >$@

INSTALL-cmakebuild.html: INSTALL-cmakebuild.md
	$(MARKDOWN) $< >$@

INSTALL-cmakebuild.txt: INSTALL-cmakebuild.html
	links -dump -width 80 -codepage utf-8 $< >$@

EXTRA_DIST += \
	INSTALL-cmakebuild.md \
	INSTALL.cmakebuild \
	CMakeLists.txt \
	doc/CMakeLists.txt \
	lib/CMakeLists.txt \
	po/CMakeLists.txt \
	src/CMakeLists.txt \
	src/config.cmake.h.in

EXTRA_DIST += COMPLIANCE

EXTRA_DIST += VERSION

DISTCLEANFILES += VERSION

VERSION:
	echo -e "PACKAGE=$(PACKAGE)\nVERSION=$(VERSION)" >$@

EXTRA_DIST += icewm.spec.in icewm.spec icewm.lsm.in icewm.lsm

DISTCLEANFILES += icewm.spec icewm.lsm

icewm.spec: icewm.spec.in
	sed -r -e 's:%%PACKAGE%%:$(PACKAGE):g;s:%%VERSION%%:$(VERSION):g;s:%%DATE%%:$(DATE):g' $< >$@

icewm.lsm: icewm.lsm.in
	sed -r -e 's:%%PACKAGE%%:$(PACKAGE):g;s:%%VERSION%%:$(VERSION):g;s:%%DATE%%:$(DATE):g' $< >$@

dist_bin_SCRIPTS = \
	icewm-set-gnomewm

