PGM = cf10.0b3
DOCDIR = docs
RHESSYS_BIN = /usr/local/bin
CC  = gcc
INCLUDES = -Iinclude
CFLAGS = -I$(GISBASE)/include -g -Wall -std=c99
CFLAGS_TESTS = `pkg-config --cflags glib-2.0` -g -Wall -std=c99
LDLIBS = -L$(GISBASE)/lib -lm -lgrass_gis
LDLIBS_TESTS = `pkg-config --libs glib-2.0` -L$(GISBASE)/lib -lm -lgrass_gis

ifdef COVERAGE
CFLAGS += -fprofile-arcs -ftest-coverage
CFLAGS_TESTS += -fprofile-arcs -ftest-coverage
LDLIBS += -lgcov
LDLIBS_TESTS += -lgcov
endif
COVERAGE_FILES = *.gcov

#SRCDIR = src
#SRCS := $(shell find $(SRCDIR) -name '*.c')
#OBJDIR = objects
OBJECTS := $(patsubst $(SRCDIR)/%.c,$(OBJDIR)/%.o,$(SRCS))

#TESTS_ROOTDIR = test
#SRCDIR_TESTS = $(TESTS_ROOTDIR)/src
#SRCS_TESTS := $(shell find $(SRCDIR_TESTS) -name '*.c')
TESTSDIR = $(TESTS_ROOTDIR)
#OBJDIR_TESTS = $(TESTS_ROOTDIR)/objects
OBJECTS_TESTS := $(patsubst $(SRCDIR_TESTS)/%.c,$(OBJDIR_TESTS)/%.o,$(SRCS_TESTS))
TESTS := $(patsubst $(SRCDIR_TESTS)/%.c,$(OBJDIR_TESTS)/%,$(SRCS_TESTS))
OBJECTS_NO_MAIN := $(patsubst $(OBJDIR)/main.o,,$(OBJECTS))
TESTS_TO_RUN = $(shell find $(TESTS_ROOTDIR) -type f -perm +111 -maxdepth 1)

ifneq ($(OS), Windows_NT)
	OS := $(shell uname)
endif

#all: dir $(PGM)

#dir:
#	mkdir -p $(DOCDIR)
#	mkdir -p $(OBJDIR)
#	mkdir -p $(OBJDIR_TESTS)


$(PGM): $(OBJECTS)
ifeq ($(OS), Darwin)
	$(CC) $(OBJECTS) $(CFLAGS) $(INCLUDES) $(LDLIBS) -o $(PGM)
else ifeq ($(OS), Linux)
	$(CC) $(OBJECTS) $(CFLAGS) $(INCLUDES) $(LDLIBS) -lbsd -Wl,-rpath=$(GISBASE)/lib -o $(PGM)
else ifeq ($(OS), Windows_NT)
	$(CC) $(OBJECTS) $(CFLAGS) $(INCLUDES) $(LDLIBS),-rpath=$(GISBASE)/lib -o $(PGM)
else
	$(CC) $(OBJECTS) $(CFLAGS) $(INCLUDES) $(LDLIBS) -Wl,-rpath=$(GISBASE)/lib -o $(PGM)
endif

$(OBJDIR)/%.o: $(SRCDIR)/%.c
	$(CC) $(CFLAGS) $(INCLUDES) -c -o $@ $< 

objects: $(OBJECTS)

test: $(TESTS) 
	# Run each unit test
	$(patsubst %,%;,$(TESTS_TO_RUN))

$(TESTS): $(OBJECTS) $(OBJECTS_TESTS) 
ifeq ($(OS), Darwin)
	$(CC) $(OBJECTS_NO_MAIN) $@.o $(CFLAGS_TESTS) $(INCLUDES) $(LDLIBS_TESTS) -o $@
else ifeq ($(OS), Linux)
	$(CC) $(OBJECTS_NO_MAIN) $@.o $(CFLAGS_TESTS) $(INCLUDES) $(LDLIBS_TESTS) -lbsd -Wl,-rpath=$(GISBASE)/lib -o $@
else ifeq ($(OS), Windows_NT)
	$(CC) $(OBJECTS_NO_MAIN) $@.o $(CFLAGS_TESTS) $(INCLUDES) $(LDLIBS_TESTS) -lbsd -Wl,-rpath=$(GISBASE)/lib -o $@
else
	$(CC) $(OBJECTS_NO_MAIN) $@.o $(CFLAGS_TESTS) $(INCLUDES) $(LDLIBS_TESTS) -Wl,-rpath=$(GISBASE)/lib -o $@
endif
	cp $@ $(TESTS_ROOTDIR)

$(OBJDIR_TESTS)/%.o: $(SRCDIR_TESTS)/%.c
	$(CC) $(CFLAGS_TESTS) $(INCLUDES) -c -o $@ $< 

functest: $(PGM)
	# Run Python-based functional testing
	CF_BIN=$(PGM) LD_LIBRARY_PATH=$(GISBASE)/lib python -m unittest discover -s test

doc:
	doxygen

install: $(PGM)
ifeq ($(OS), Windows_NT)
	copy $(PGM) $(RHESSYS_BIN)
else
	cp $(PGM) $(RHESSYS_BIN)
endif

clean:
ifeq ($(OS), Windows_NT)
	del $(PGM)
	rmdir /s $(OBJECTS) $(TESTS) $(OBJECTS_TESTS) $(TESTS_TO_RUN) $(COVERAGE_FILES)
else
	rm -f $(PGM) $(OBJECTS) $(TESTS) $(OBJECTS_TESTS) $(TESTS_TO_RUN) $(COVERAGE_FILES)
endif

docclean:
ifeq ($(OS), Windows_NT)
	rmdir /s $(DOCDIR)/html
else
	rm -rf $(DOCDIR)/html
endif

