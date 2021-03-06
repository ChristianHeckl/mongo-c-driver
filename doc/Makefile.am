MAN3 = $(MONGOC_SYMBOLS)

$(top_srcdir)/doc/mongoc_symbols.txt: $(top_srcdir)/src/libmongoc.symbols
	sed -e 's/.*/linkmongoc:&[3] +/' < $< > $@

$(top_srcdir)/doc/mongoc_api.txt: $(top_srcdir)/doc/mongoc_api
	sed -e 's/.*/linkmongoc:&[7] +/' < $< > $@

MAN7 = $(MONGOC_API)

MAN_DOC = $(MAN1) $(MAN3) $(MAN7)

MAN_TXT = \
	$(MAN3:%.3=%.txt) \
	$(MAN7:%.7=%.txt)

MAN_HTML = $(MAN_TXT:%.txt=%.html)

if INSTALL_MAN
dist_man_MANS = $(MAN_DOC)
endif

EXTRA_DIST += $(top_srcdir)/doc/asciidoc.conf $(MAN_TXT)
if BUILD_DOC
EXTRA_DIST += $(MAN_HTML)
endif

MAINTAINERCLEANFILES = $(MAN_DOC) $(MAN_HTML)

doc : \
	$(top_srcdir)/doc/mongoc_api.txt \
	$(top_srcdir)/doc/mongoc_symbols.txt \
	$(top_srcdir)/doc/asciidoc.conf \
	$(DOC_EXAMPLES) \
	$(MAN_DOC) \
	$(MAN_HTML)

dist-hook : doc

if BUILD_DOC
SUFFIXES=.html .txt .xml .3 .7

EXAMPLE_DIR = $(abs_top_srcdir)/examples

.txt.html:
	if [ -L $< ]; then \
	   ln -fs `basename \`readlink $<\` .txt`.html $@; \
	else \
	   asciidoc -d manpage -b xhtml11 -f $(top_srcdir)/doc/asciidoc.conf \
	      -adocs=$(abs_top_srcdir)/doc \
	      -aexamples=$(EXAMPLE_DIR) -amongoc_version=@PACKAGE_VERSION@ -o$@ $<; \
	fi
.txt.xml:
	if [ -L $< ]; then \
	   ln -fs `basename \`readlink $<\` .txt`.xml $@; \
	else \
	   asciidoc -d manpage -b docbook -f $(top_srcdir)/doc/asciidoc.conf \
	      -adocs=$(abs_top_srcdir)/doc \
	      -aexamples=$(EXAMPLE_DIR) -amongoc_version=@PACKAGE_VERSION@ \
	      -amongoc_manname=$(*F) -o$@ $<; \
	fi
.xml.3:
	if [ -L $< ]; then \
	   ln -fs `basename \`readlink $<\` .xml`.3 $@; \
        else \
	   xmlto man -o $(top_srcdir)/doc $<; \
	fi
.xml.7:
	if [ -L $< ]; then \
	   ln -fs `basename \`readlink $<\` .xml`.3 $@; \
        else \
	   xmlto man -o $(top_srcdir)/doc $<; \
        fi
endif
