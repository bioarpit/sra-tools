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
MODULE = tools/rd-filter-redact

include $(TOP)/build/Makefile.env

INT_TOOLS = \
	test-read-write-cursor

EXT_TOOLS = \
	read-filter-redact

ALL_TOOLS = \
	$(INT_TOOLS) \
	$(EXT_TOOLS)

#------------------------------------------------------------------------------
# outer targets
#
all std: vers-includes
	@ $(MAKE_CMD) $(TARGDIR)/$@

$(ALL_TOOLS): vers-includes
	@ $(MAKE_CMD) $(BINDIR)/$@

.PHONY: all std $(ALL_TOOLS)

#-------------------------------------------------------------------------------
# all
#
$(TARGDIR)/all: \
	$(addprefix $(BINDIR)/,$(ALL_TOOLS))

.PHONY: $(TARGDIR)/all

#-------------------------------------------------------------------------------
# std
#
$(TARGDIR)/std: \
	$(addprefix $(BINDIR)/,$(EXT_TOOLS))

.PHONY: $(TARGDIR)/std

#-------------------------------------------------------------------------------
# vers-includes
#
$(TARGDIR)/vers-includes: $(addsuffix .vers.h,$(EXT_TOOLS))

.PHONY: $(TARGDIR)/vers-includes

#-------------------------------------------------------------------------------
# clean
#
clean: stdclean

.PHONY: clean

#-------------------------------------------------------------------------------
# read-filter-redact
#
READ_FILTER_REDACT_SRC = \
	read-filter-redact \

READ_FILTER_REDACT_OBJ = \
	$(addsuffix .$(OBJX),$(READ_FILTER_REDACT_SRC))

READ_FILTER_REDACT_LIB = \
	-lkapp \
	-sncbi-wvdb \
	-lm

read-filter-redact.vers.h: read-filter-redact.vers
	$(TOP)/build/make-vers-inc.sh READ_FILTER_REDACT_VERS $^ > $@

$(BINDIR)/read-filter-redact: $(READ_FILTER_REDACT_OBJ)
	$(LD) --exe --vers $(SRCDIR) -o $@ $^ $(READ_FILTER_REDACT_LIB)

#-------------------------------------------------------------------------------
# test-read-write-cursor
#
TEST_READ_WRITE_CURSOR_SRC = \
	test-read-write-cursor \

TEST_READ_WRITE_CURSOR_OBJ = \
	$(addsuffix .$(OBJX),$(TEST_READ_WRITE_CURSOR_SRC))

TEST_READ_WRITE_CURSOR_LIB = \
	-lkapp \
	-lncbi-wvdb \

test-read-write-cursor.vers.h: test-read-write-cursor.vers
	$(TOP)/build/make-vers-inc.sh TEST_READ_WRITE_CURSOR_VERS $^ > $@

$(BINDIR)/test-read-write-cursor: $(TEST_READ_WRITE_CURSOR_OBJ)
	$(LD) --exe --vers $(SRCDIR) -o $@ $^ $(TEST_READ_WRITE_CURSOR_LIB)

