# ===========================================================================
#
#                            PUBLIC DOMAIN NOTICE
#               National Center for Biotechnology Information
#
#  This software/database is a "United States Government Work" under the
#  terms of the United States Copyright Act.  It was written as part of
#  the author's official duties as a United States Government employee and
#  thus cannot be copyrighted.  This software/database is freely available
#  to the public for use. The National Library of Medicine and the U.S.
#  Government have not placed any restriction on its use or reproduction.
#
#  Although all reasonable efforts have been taken to ensure the accuracy
#  and reliability of the software and data, the NLM and the U.S.
#  Government do not and cannot warrant the performance or results that
#  may be obtained by using this software or data. The NLM and the U.S.
#  Government disclaim all warranties, express or implied, including
#  warranties of performance, merchantability or fitness for any particular
#  purpose.
#
#  Please cite the author in any work or product based on this material.
#
# ===========================================================================

default: std

TOP ?= $(abspath ../..)
MODULE = tools/fastq-loader

INT_LIBS = \
	libfastqloader

ALL_LIBS = \
	$(INT_LIBS) \
	$(EXT_LIBS)

EXT_TOOLS = \
    latf-load

ALL_TOOLS = \
	$(INT_TOOLS) \
	$(EXT_TOOLS)    

include $(TOP)/build/Makefile.env

#-------------------------------------------------------------------------------
# outer targets
#
all std: vers-includes 
	@ $(MAKE_CMD) $(TARGDIR)/std

$(INT_LIBS): vers-includes
	@ $(MAKE_CMD) $(ILIBDIR)/$@

$(EXT_TOOLS): vers-includes
	@ $(MAKE_CMD) $(BINDIR)/$@    

ALL_TOOLS = \
	$(INT_TOOLS) \
	$(EXT_TOOLS)

.PHONY: all std $(ALL_LIBS) 

#-------------------------------------------------------------------------------
# std
#
$(TARGDIR)/std: \
	$(addprefix $(ILIBDIR)/,$(INT_LIBS)) \
	$(addprefix $(LIBDIR)/,$(EXT_LIBS)) \
	$(addprefix $(BINDIR)/,$(ALL_TOOLS))

.PHONY: $(TARGDIR)/std

#-------------------------------------------------------------------------------
# vers-includes
#
$(TARGDIR)/vers-includes: $(addsuffix .vers.h,$(ALL_TOOLS))

.PHONY: $(TARGDIR)/vers-includes

#-------------------------------------------------------------------------------
# clean
#
INTERM_SRC = \
	$(SRCDIR)/fastq-lex.c \
	$(SRCDIR)/fastq-grammar.c \
	$(SRCDIR)/fastq-tokens.h 

clean: stdclean

.PHONY: clean

.PRECIOUS: $(INTERM_SRC)

#-------------------------------------------------------------------------------
# fastq
#
$(ILIBDIR)/libfastqloader: $(ILIBDIR)/libfastqloader.$(LIBX)

FASTQ_SRC = \
    fastq-reader \
	fastq-grammar \
	fastq-lex

# flex/bison should only be invoked manually in an environment ensures the correct versions:
# bison 2.5, flex 2.5.35
bison:
	bison -o $(SRCDIR)/fastq-grammar.c --defines=$(SRCDIR)/fastq-tokens.h -v --no-lines $(SRCDIR)/fastq-grammar.y

flex:    
	flex -DYY_BUF_SIZE=16777216 -t $(SRCDIR)/fastq-lex.l | grep -v '^#line' > $(SRCDIR)/fastq-lex.c

FASTQ_OBJ = \
	$(addsuffix .$(LOBX),$(FASTQ_SRC))

FASTQ_LIB = \
    -lload \
    -lloader \
	-dkfs \
	-dklib \

ifneq (win,$(OS))
    FASTQ_LIB += -dkq
endif

$(ILIBDIR)/libfastqloader.$(SHLX): $(INTERM_SRC) $(FASTQ_OBJ)
	$(LD) --dlib -o $@ $^ $(FASTQ_LIB)

$(ILIBDIR)/libfastqloader.$(LIBX): $(INTERM_SRC) $(FASTQ_OBJ)
	$(LD) --slib -o $@ $^ $(FASTQ_LIB)

#------------------------------------------------------------------------------
# fastq-load.3
#
FASTQ_LOAD_SRC = \
	fastq-loader \
    loader-imp \
	$(FASTQ_SRC)

FASTQ_LOAD_OBJ = \
	$(addsuffix .$(OBJX),$(FASTQ_LOAD_SRC))

FASTQ_LOADER_LIB = \
	-skapp \
    -sloader \
    -sload \
	-sncbi-wvdb \
	-sm

$(BINDIR)/latf-load: $(FASTQ_LOAD_OBJ)
	$(LD) --exe --vers $(SRCDIR)/latf-load.vers -o $@ $^ $(FASTQ_LOADER_LIB)

