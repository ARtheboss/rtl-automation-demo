# FPU Simulation Makefile for Icarus Verilog

# Directories
SRC_DIR = ../src
SIM_DIR = .

# Source files
SRC_FILES = $(SRC_DIR)/fpu_pkg.v \
            $(SRC_DIR)/fpu_normalize.v \
            $(SRC_DIR)/fpu_add.v \
            $(SRC_DIR)/fpu_mul.v \
            $(SRC_DIR)/fpu_top.v

# Compiler flags
IVERILOG = iverilog
VVP = vvp
IVFLAGS = -Wall -g2012

# Default target
.PHONY: all
all: test_top

# Run all tests
.PHONY: test
test: test_add test_mul test_top

# Run all tests with FST output
.PHONY: test_fst
test_fst: test_add_fst test_mul_fst test_top_fst

# Top-level FPU test
.PHONY: test_top
test_top: fpu_top_sim
	$(VVP) fpu_top_sim

# Top-level FPU test with FST output
.PHONY: test_top_fst
test_top_fst: fpu_top_sim
	$(VVP) fpu_top_sim +dumpfile=fpu_sim.fst

fpu_top_sim: $(SRC_FILES) tb_fpu_top.v
	$(IVERILOG) $(IVFLAGS) -I$(SRC_DIR) -o $@ $(SRC_FILES) tb_fpu_top.v

# Adder unit test
.PHONY: test_add
test_add: fpu_add_sim
	$(VVP) fpu_add_sim

# Adder unit test with FST output
.PHONY: test_add_fst
test_add_fst: fpu_add_sim
	$(VVP) fpu_add_sim +dumpfile=fpu_add_sim.fst

fpu_add_sim: $(SRC_DIR)/fpu_pkg.v $(SRC_DIR)/fpu_add.v tb_fpu_add.v
	$(IVERILOG) $(IVFLAGS) -I$(SRC_DIR) -o $@ $(SRC_DIR)/fpu_pkg.v $(SRC_DIR)/fpu_add.v tb_fpu_add.v

# Multiplier unit test
.PHONY: test_mul
test_mul: fpu_mul_sim
	$(VVP) fpu_mul_sim

# Multiplier unit test with FST output
.PHONY: test_mul_fst
test_mul_fst: fpu_mul_sim
	$(VVP) fpu_mul_sim +dumpfile=fpu_mul_sim.fst

fpu_mul_sim: $(SRC_DIR)/fpu_pkg.v $(SRC_DIR)/fpu_mul.v tb_fpu_mul.v
	$(IVERILOG) $(IVFLAGS) -I$(SRC_DIR) -o $@ $(SRC_DIR)/fpu_pkg.v $(SRC_DIR)/fpu_mul.v tb_fpu_mul.v

# Compile only (no run)
.PHONY: compile
compile: fpu_top_sim fpu_add_sim fpu_mul_sim
	@echo "All modules compiled successfully"

# View waveforms (requires GTKWave)
.PHONY: wave
wave: test_top
	gtkwave fpu_sim.vcd &

.PHONY: wave_add
wave_add: test_add
	gtkwave fpu_add_sim.vcd &

.PHONY: wave_mul
wave_mul: test_mul
	gtkwave fpu_mul_sim.vcd &

# View FST waveforms (requires GTKWave)
.PHONY: wave_fst
wave_fst: test_top_fst
	gtkwave fpu_sim.fst &

.PHONY: wave_add_fst
wave_add_fst: test_add_fst
	gtkwave fpu_add_sim.fst &

.PHONY: wave_mul_fst
wave_mul_fst: test_mul_fst
	gtkwave fpu_mul_sim.fst &

# Lint check
.PHONY: lint
lint:
	$(IVERILOG) $(IVFLAGS) -I$(SRC_DIR) -o /dev/null $(SRC_FILES)
	@echo "Lint check passed"

# Clean
.PHONY: clean
clean:
	rm -f fpu_top_sim fpu_add_sim fpu_mul_sim
	rm -f *.vcd
	rm -f *.lxt *.lxt2
	rm -f *.fst

# Help
.PHONY: help
help:
	@echo "FPU Simulation Makefile"
	@echo ""
	@echo "Targets:"
	@echo "  all          - Build and run top-level test (default)"
	@echo "  test         - Run all tests (VCD output)"
	@echo "  test_fst     - Run all tests (FST output)"
	@echo "  test_top     - Run top-level FPU test"
	@echo "  test_add     - Run adder unit test"
	@echo "  test_mul     - Run multiplier unit test"
	@echo "  test_top_fst - Run top-level test with FST output"
	@echo "  test_add_fst - Run adder test with FST output"
	@echo "  test_mul_fst - Run multiplier test with FST output"
	@echo "  compile      - Compile all without running"
	@echo "  lint         - Check for syntax/lint errors"
	@echo "  wave         - Run test and open VCD waveform"
	@echo "  wave_fst     - Run test and open FST waveform"
	@echo "  clean        - Remove generated files"
	@echo "  help         - Show this help"
