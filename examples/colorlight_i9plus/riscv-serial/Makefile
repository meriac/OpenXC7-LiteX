.PHONY: all clean

PROJECT:=colorlight_i9plus
BUILD_DIR:=build

BIT_FILE:=$(BUILD_DIR)/gateware/$(PROJECT).bit
CHIPDB:=$(PWD)

all: $(BIT_FILE)

# Compile Python LiteX HD into a flat Verilog file and that to a FPGA bit stream
$(BIT_FILE): generate.py colorlight_i9plus.py
	@rm -rf $(BUILD_DIR)
	python3 $< --toolchain openxc7 --output-dir $(BUILD_DIR) --build
	@ls -l $@

clean:
	@rm -rf $(BUILD_DIR)

clean_all: clean
	@rm -f *.bin
	@rm -rf __pycache__
