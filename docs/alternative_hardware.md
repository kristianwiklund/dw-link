An alternative hardware solution is described here: [AVR/Arduino Hardware Debugger on the Cheap](https://sites.google.com/site/wayneholder/debugwire3) which I (https://github.com/kristianwiklund/) happened to have built before I found this project

* RESET is connected to D10
* POWER is controlled by D9 - D9 high enables power to the device-under-test
* MOSI is connected to D11
* MISO is connected to D12
* SCK is connected to D13

We have the following functional pins in the manual:

AH Mapping | Functional pin | Direction | Explanation
--- | --- | --- | ---
NC | DEBTX | Output | Serial line for debugging output (when debugging the debugger) 
D10 | DWLINE | Input/Output | debugWIRE communication line to be connected to RESET on the target board
GND | GND | Supply | Ground
NC  | TISP | Output | Control line: If low, then ISP programming is enabled 
D12 | TMISO | Input | SPI signal "Master In, Slave Out"
D11 | TMOSI | Output | SPI signal "Master Out, Slave In"
D13 | TSCK | Output | SPI signal "Master clock"
D1  | SNSGND | Input | If open, signals that the debugger should use the [pin mapping tuned for an ISP cable](#simplemap), if low, use the pin mapping for the particular board as specified in the [table below](#complexmap) 
NC  | V33 | Output | Control line to the MOSFET to switch on 3.3 volt supply for target
NC  | V5  | Output | Control line to switch on the 5 volt line
Vcc | Vcc | Supply | Voltage supply from the board (5 V) that can be used to power the target
VHIGH | Input from switch | If low, then choose 5 V supply for target, otherwise 3.3 V 
VON | Input from switch | If low, then supply target (and use power-cycling)
VSUP | Output | Used as a target supply line driven directly by an ATmega pin, i.e., this should not source more than 20 mA (this is the cheap alternative to using MOSFETs to drive the supply for the target board, e.g., when using a prototype board as sketched in [Section 7.1](#section71)) 

We redefine the pin map to
	// VHIGH, VON, V33, TSCK, TMOSI, TMISO, VSUP, DEBTX, TISP, SYSLED, LEDGND
    const PROGMEM pinmap  boardpm  = { 2, 5, 9, 7, 12, 10, 11, 15, 3, 6, 13, pundef } ;
    const byte SNSGND = 14;
    const byte DWLINE = 10; 

which is enabled by #defining ALTERNATE_BOARD_AHDOTC in the source code, and it should work.

