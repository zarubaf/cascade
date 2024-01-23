#
# To use this example, the cascade library must be built with
#
#   env VERILOG=1 make
#
RM := /bin/rm -rf

VCS     := $(VCS_HOME)/bin/vcs
CXX		:= $(VCS_HOME)/amd64/clang/bin/clang++
CFLAGS  := -ggdb3 -Wall -std=gnu++0x -I../../include
VFLAGS  := -sverilog -full64 +incdir+../../verilog

LIBHPPFILES := $(wildcard ../../include/*/*.hpp)

LIBDESCORE := ../../build/libdescore.a
LIBCASCADE := ../../build/libcascade_verilog.a

simv: $(LIBDESCORE) $(LIBCASCADE) Adder.o tb_adder.sv CAdder.sv
	$(VCS) $(VFLAGS) Adder.o CAdder.sv tb_adder.sv $(LIBCASCADE) $(LIBDESCORE) -lz

Adder.o: Adder.cpp $(LIBHPPFILES)
	$(CXX) $(CFLAGS) -D_VERILOG -D_GLIBCXX_USE_CXX11_ABI=0 -I$(VCS_HOME)/include -c -o Adder.o Adder.cpp

$(LIBDESCORE):
	cd ../../src/descore; make

$(LIBCASCADE):
	cd ../../src/cascade; make verilog

verilate:
	verilator -I../../verilog --binary tb_adder.sv CAdder.sv ../../../build/libadder.a ../../../build/libcascade.a ../../../build/libdescore.a

clean:
	$(RM) simv
	$(RM) Adder.o
	$(RM) csrc
	$(RM) simv.daidir
	$(RM) ucli.key
