DESTDIR ?=
PREFIX ?= /usr/local
MANDIR ?= $(PREFIX)/share/man

DOXYGEN := $(shell command -v doxygen 2> /dev/null)
DOXYGENFLAGS =

all: Doxyfile
ifndef DOXYGEN
	$(warning "doxygen is not available please install it")
else
	$(DOXYGEN) $(DOXYGENFLAGS)
endif

Doxyfile: Doxyfile.in
	@sed \
	  -e 's%@MAINPAGE_H@%./mainpage.h%' \
	  -e 's%@SRCDIR@%../src%' \
	  -e 's%@INCLUDEDIR@%../include/tinyalsa%' \
	  -e 's%@EXAMPLE_DIR@%../examples%' \
	  -e 's%@HTML_OUTPUT_DIR@%html%' \
	  -e 's%@MAN_OUTPUT_DIR@%man%' \
	  -e 's%@MAN_OUTPUT_SUBDIR@%%' \
	  -e 's%@GENERATE_HTML@%YES%' \
	  -e 's%@GENERATE_MAN@%YES%' \
	  $^ > $@

.PHONY: clean
clean:
ifdef DOXYGEN
	rm -Rf html
	rm -Rf man
endif

.PHONY: install
install:
ifdef DOXYGEN
	install -d $(DESTDIR)$(MANDIR)/man3
	install man/man3/libtinyalsa-pcm.3 $(DESTDIR)$(MANDIR)/man3
	install man/man3/libtinyalsa-mixer.3 $(DESTDIR)$(MANDIR)/man3
endif
