DB_DIR=${PREFIX}/share/nextpnr/external/prjxray-db

ARCH:=artix7
BOARD:=arty
PART:=xc7a50tfgg484-1
PROJECT:=blinky

ZIP_FILE:=$(PROJECT)-bitstream.zip
BIT_FILE:=$(PROJECT).bit
CHIPDB:=$(PWD)

.PHONY: all
all: $(ZIP_FILE)

.PHONY: program
program: $(PROJECT).bit
	openFPGALoader --board $(BOARD) --bitstream $<

$(ZIP_FILE): $(BIT_FILE)
	zip -j9 $@ $^

$(PROJECT).json: $(PROJECT).v
	yosys -p "synth_xilinx -flatten -abc9 -nobram -arch xc7 -top $(PROJECT); write_json $@" $<

# The chip database only needs to be generated once
# that is why we don't clean it with make clean
${CHIPDB}/${PART}.bin:
	pypy3 $(PREFIX)/share/nextpnr/python/bbaexport.py --device ${PART} --bba ${PART}.bba
	bbasm -l ${PART}.bba $@
	rm -f ${PART}.bba

$(PROJECT).fasm: $(PROJECT).json ${CHIPDB}/${PART}.bin $(PROJECT).xdc
	nextpnr-xilinx --chipdb ${CHIPDB}/${PART}.bin --xdc $(PROJECT).xdc --json $< --fasm $@ --verbose --debug

$(PROJECT).frames: $(PROJECT).fasm
	fasm2frames --part ${PART} --db-root ${DB_DIR}/$(ARCH) $< > $@

$(BIT_FILE): $(PROJECT).frames
	xc7frames2bit --part_file ${DB_DIR}/$(ARCH)/${PART}/part.yaml --part_name ${PART} --frm_file $< --output_file $@

.PHONY: clean
clean:
	@rm -f $(ZIP_FILE) $(PROJECT).bit $(PROJECT).frames $(PROJECT).fasm $(PROJECT).json

.PHONY: clean_all
clean_all: clean
	@rm -f ${CHIPDB}/${PART}.bin