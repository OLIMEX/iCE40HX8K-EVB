PROJ = example
PIN_DEF = ice40hx8k-evb.pcf
DEVICE = hx8k

all: $(PROJ).rpt $(PROJ).bin

%.json: %.v
	yosys -p 'synth_ice40 -top top -json $@' $<

%.asc: %.json $(PIN_DEV)
	nextpnr-ice40 --hx8k --json $< --pcf $(PIN_DEF) --asc $@

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
	rm -f $(PROJ).json $(PROJ).asc $(PROJ).bin $(PROJ).rpt

.PHONY: all prog clean
