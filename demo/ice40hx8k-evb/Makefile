PROJ = example
PIN_DEF = ice40hx8k-evb.pcf
DEVICE = hx8k
PACKAGE = ct256

all: $(PROJ).rpt $(PROJ).bin

%.blif %.json : %.v
	yosys -p 'synth_ice40 -top top -blif $@ -json $*.json' $<

%.asc: $(PIN_DEF) %.blif %.json
	nextpnr-ice40 --$(DEVICE) --package $(PACKAGE) --json $*.json --asc $@ --pcf $(PIN_DEF)

%.bin: %.asc
	icepack $< $@

%.rpt: %.asc
	icetime -d $(DEVICE) -mtr $@ $<

prog: $(PROJ).bin
	sudo iceprogduino $<

sudo-prog: $(PROJ).bin
	@echo 'Executing prog as root!!!'
	sudo iceprogduino $<

clean:
	rm -f $(PROJ).blif $(PROJ).asc $(PROJ).bin $(PROJ).rpt
 
.PHONY: all prog clean
