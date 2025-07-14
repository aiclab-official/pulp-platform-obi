# Vivado Makefile for PULP OBI simulation

# Adjust path if Makefile is in vivado/, relative to project root
INCDIRS := -i ../include -i ../../common_cells/include

SRCS := \
  ../../common_cells/src/cf_math_pkg.sv \
  ../../common_cells/src/addr_decode.sv \
  ../../common_cells/src/delta_counter.sv \
  ../../common_cells/src/lzc.sv \
  ../../common_cells/src/addr_decode_dync.sv \
  ../../common_cells/src/rr_arb_tree.sv \
  ../../common_cells/src/fifo_v3.sv \
  ../../common_verification/src/clk_rst_gen.sv \
  ../src/obi_pkg.sv \
  ../src/obi_intf.sv \
  ../src/obi_sram_shim.sv \
  ../src/obi_mux.sv \
  ../src/obi_demux.sv \
  ../src/obi_xbar.sv \
  ../src/test/obi_sim_mem.sv \
  ../src/test/obi_asserter.sv \
  ../src/test/obi_test.sv \
  ../src/test/tb_obi_xbar.sv \
  ../src/test/atop_golden_mem_pkg.sv

# Testbench (override on command line: make TB=tb_obi_xbar)
TB    ?= tb_obi_xbar
TOP   := $(TB)

# Vivado tools
XVLOG := xvlog
XELAB := xelab
XSIM  := xsim

# Simulation run script (optional)
XSIM_SCRIPT := xsim.tcl

all: sim

sim:
	$(XVLOG) --sv $(INCDIRS) $(SRCS)
	$(XELAB) $(TOP) -s $(TB)_sim -debug all
	$(XSIM) $(TB)_sim -t $(XSIM_SCRIPT)

clean:
	rm -rf .Xil .Xil_* *.jou *.log *.wdb $(TB)_sim

.PHONY: all sim clean
