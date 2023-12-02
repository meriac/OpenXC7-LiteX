.PHONY: all clean clean_all

PROJECT:=colorlight_i9plus
BUILD_DIR:=build

ZIP_FILE:=$(PROJECT)-bitstream.zip
BIT_FILE:=$(BUILD_DIR)/gateware/$(PROJECT).bit
CHIPDB:=$(PWD)

all: $(ZIP_FILE)

$(ZIP_FILE): $(BIT_FILE)
	zip -j9 $@ $^

# Compile Python LiteX HD into a flat Verilog file and that to a FPGA bit stream
$(BIT_FILE): generate.py colorlight_i9plus.py
	@rm -rf $(BUILD_DIR)
	python3 $< --toolchain openxc7 --output-dir $(BUILD_DIR) --build
	@ls -l $@

clean:
	@rm -f $(ZIP_FILE)
	@rm -rf $(BUILD_DIR)

clean_all: clean
	@rm -f *.bin
	@rm -rf __pycache__
