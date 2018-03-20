## Process this file with automake to produce Makefile.in

LANG1=tlh
BASENAME=apertium-$(LANG1)

# This include detlhes goals for install-modes, .deps/.d and .mode files:
@ap_include@

TARGETS_COMMON=$(LANG1).rlx.bin
TARGETS_COMMON+=$(LANG1).automorf.hfst $(LANG1).autogen.hfst

.deps/.d:
	$(MKDIR_P) .deps
	touch $@

$(LANG1).autogen.bin: $(BASENAME).$(LANG1).dix .deps/.d
	apertium-validate-dictionary $<
	lt-comp rl $< $@ $(BASENAME).$(LANG1).acx

$(LANG1).automorf.bin: $(BASENAME).$(LANG1).dix .deps/.d
	apertium-validate-dictionary $<
	lt-comp lr $< $@ $(BASENAME).$(LANG1).acx

.deps/$(LANG1).gen.twol.hfst: $(BASENAME).$(LANG1).gen.twol .deps/.d
	hfst-twolc --resolve $< -o $@

.deps/$(LANG1).morf.twol.hfst: $(BASENAME).$(LANG1).morf.twol .deps/.d
	hfst-twolc --resolve $< -o $@

.deps/$(LANG1).lexc.hfst: $(BASENAME).$(LANG1).lexc .deps/.d
	hfst-lexc $< -o $@ --alignStrings

$(LANG1).autogen.hfst: .deps/$(LANG1).gen.twol.hfst .deps/$(LANG1).lexc.hfst
	hfst-compose-intersect -1 .deps/$(LANG1).lexc.hfst -2 .deps/$(LANG1).gen.twol.hfst |\
		hfst-fst2fst -f olw -o $@

$(LANG1).automorf.hfst: .deps/$(LANG1).morf.twol.hfst .deps/$(LANG1).lexc.hfst 
	hfst-compose-intersect -1 .deps/$(LANG1).lexc.hfst -2 .deps/$(LANG1).morf.twol.hfst |\
		hfst-invert |\
		hfst-fst2fst -f olw -o $@

$(LANG1).rlx.bin: $(BASENAME).$(LANG1).rlx $(CGCOMP)
	$(CGCOMP) $< $@

## Distribution

EXTRA_DIST=$(BASENAME).$(LANG1).dix \
	   $(BASENAME).$(LANG1).acx \
	   $(BASENAME).$(LANG1).rlx \
	   $(BASENAME).$(LANG1).lexc \
	   $(BASENAME).$(LANG1).gen.twol $(BASENAME).$(LANG1).morf.twol \
	   coverages.bash lookups.bash lookdowns.bash \
	   modes.xml

## Installation

apertium_tlhdir=$(prefix)/share/apertium/$(BASENAME)/
apertium_tlh_srcdir=$(prefix)/share/apertium/$(BASENAME)/

apertium_tlh_DATA=$(TARGETS_COMMON)
apertium_tlh_src_DATA=$(BASENAME).$(LANG1).rlx $(BASENAME).$(LANG1).dix

pkgconfigdir = $(prefix)/share/pkgconfig
pkgconfig_DATA = $(BASENAME).pc

apertium_modesdir=$(prefix)/share/apertium/modes/
apertium_modes_DATA=modes/$(LANG1)-morph.mode

modes/tlh-morph.mode: modes.xml
	if test  x$(srcdir) != x$(builddir) ; then \
		cp -v $< . ;\
	fi
	apertium-gen-modes modes.xml


## Cleanup

CLEANFILES = $(TARGETS_COMMON)
clean-local:
	-rm -rf .deps modes *.mode misses.freqs
	if test  x$(srcdir) != x$(builddir) ; then \
		rm -f $(builddir)/modes.xml ;\
	fi

## Tests

TESTS=coverages.bash lookups.bash lookdowns.bash
