# CEM: GNU-style makefile; use gmake.
#
# Assumes pwd is unix. Also - does not provide dependency checks for .h.
ROOT = ..
BIN = $(ROOT)/bin
SRC = $(ROOT)/src
INC = $(ROOT)/include
TOOLS = $(ROOT)/tools

LIBNAME = pracrand
LIBFILE = lib$(LIBNAME).a
LIB = $(BIN)/lib$(LIBNAME).a

BINS = RNG_benchmark RNG_output RNG_test Test_calibration
TARGETS = $(LIB) $(addprefix $(BIN)/,$(BINS))

INCLUDES = $(INC) $(TOOLS)
LIBS = $(BIN)/libpracrand

HDRS = $(wildcard $(ROOT)/**/*.h)

CXX ?= c++
CXXFLAGS += -g $(addprefix -I,$(INCLUDES)) -O2 -std=c++11 -pthread
CXXFLAGS += -march=native

CXXFLAGS += -Wall -Wno-unused-variable -Wno-unneeded-internal-declaration
CXXFLAGS += -Wno-unused-local-typedef

# Not especially specific, but force rebuild if headers change.  Certainly some
# headers have impact on some object files, and I have punted on mapping the
# granularity out more specifically.
%.o: %.cpp $(HDRS)
	$(CXX) -o $@ $(CXXFLAGS) -c $<

$(BIN)/%: $(TOOLS)/%.o $(LIB)
	$(CXX) -o $@ $^ $(CXXFLAGS)

all: $(TARGETS)

SRC_OBJS = \
	math.o \
	non_uniform.o \
	platform_specifics.o \
	rand.o \
	sha2.o \
	test_batteries.o \
	tests.o \
	RNGs/arbee.o \
	RNGs/chacha.o \
	RNGs/efiix.o \
	RNGs/hc256.o \
	RNGs/isaac.o \
	RNGs/jsf.o \
	RNGs/mt19937.o \
	RNGs/salsa.o \
	RNGs/sfc.o \
	RNGs/sha2_based_pool.o \
	RNGs/trivium.o \
	RNGs/xsm.o \
	RNGs/other/fibonacci.o \
	RNGs/other/indirection.o \
	RNGs/other/mult.o \
	RNGs/other/simple.o \
	RNGs/other/transform.o

LIB_OBJS = \
	$(addprefix $(SRC)/,$(SRC_OBJS))

$(BIN)/libpracrand.a: $(LIB_OBJS)
	ar -r $@  $^
	ranlib $@

.PHONY: clean
clean:
	rm -f $(TARGETS)
	find $(ROOT) \( -name "*~" -o -name "*.o" \) -exec rm -f {} \;
