# Main Makefile for BBDB


TAR = gtar
COMPRESS = gzip --verbose --best
COMPEXT = gz
RM   = /bin/rm -f
LN_S = ln -s

ETAGS = etags
EMACS_PROG = /Applications/Emacs.app/Contents/MacOS/Emacs -no-site-file -no-init-file

SUBDIRS = lisp texinfo tex utils

TARFILES = ChangeLog INSTALL Makefile README                        \
           aclocal.m4 configure configure.ac install-sh Makefile.in \
           bits lisp misc tex texinfo utils

all: Makefile bbdb info    gnus

Makefile: configure Makefile.in
	@echo "Makefile.in or configure has changed. Please rerun configure!"
	exit 1

configure: configure.ac
	@echo "configure.ac has changed. Please rerun autoconf!"
	exit 1

bbdb:
	cd lisp; $(MAKE) bbdb

rmail:
	cd lisp; $(MAKE) rmail

vm:
	cd lisp; $(MAKE) vm

mhe:
	cd lisp; $(MAKE) mhe

gnus:
	cd lisp; $(MAKE) gnus

info:
	cd texinfo; $(MAKE) info

autoloads:
	cd lisp; $(MAKE) autoloads

autoloadsc:
	cd lisp; $(MAKE) autoloadsc

install-pkg:
	@for i in $(SUBDIRS) ; do             \
          ( cd $$i && $(MAKE) install-pkg ) ; \
        done

uninstall-pkg:
	@for i in $(SUBDIRS) ; do               \
          ( cd $$i && $(MAKE) uninstall-pkg ) ; \
        done

clean:
	-$(RM) loadpath.el
	cd lisp; $(MAKE) clean
	cd texinfo; $(MAKE) clean

distclean:
	@for i in $(SUBDIRS) ; do           \
          ( cd $$i && $(MAKE) distclean ) ; \
        done
	-$(RM) config.cache config.status config.log
	-$(RM) -r autom4te.cache
	-$(RM) *.tar.gz
	-$(RM) .#*

# Backward compatibility
reallyclean: distclean

cvsclean:
	@for i in $(SUBDIRS) ; do          \
          ( cd $$i && $(MAKE) cvsclean ) ; \
        done
	-$(RM) Makefile configure


# tarball
tar: $(TARFILES)
	@if test "x$(TAR)" = "x" ; then                                        \
          echo "Sorry, no \`tar' program available." ;                     \
        else                                                               \
          name=`sed -n 's/^(defconst bbdb-version "\([0-9]\.[0-9][0-9]*\).*/bbdb-\1/p' lisp/bbdb.el` ;                                                    \
          $(RM) $${name} ; ln -s . $${name} ;                             \
          if test "x$(COMPRESS)" = "x" ; then                              \
            echo "Sorry, no compression program available." ;              \
            echo "The tarball will not be compressed." ;                   \
            echo "creating tar file $${name}.tar..." ;                     \
            $(TAR) --exclude=CVS --exclude=testing  --exclude=".*"         \
            -vchf $${name}.tar                                             \
            `echo $(TARFILES) | sed "s|^|$${name}/|g; s| | $${name}/|g"` ; \
          else                                                             \
            echo "creating tar file $${name}.tar.$(COMPEXT)..." ;          \
            $(TAR) --exclude=CVS  --exclude=".*" -vchf - `echo $(TARFILES) \
            | sed "s|^|$${name}/|g; s| | $${name}/|g"`                     \
            | $(COMPRESS) > $${name}.tar.$(COMPEXT) ;                      \
          fi ;                                                             \
          $(RM) $${name} ;                                                 \
        fi

dist: distclean info autoloads tar

TAGS: tags

tags:
	@if test "x$(ETAGS)" = "x" ; then                 \
           echo "Sorry, no \`etags' program available." ; \
         else                                             \
           $(ETAGS) */*.el                              ; \
         fi
