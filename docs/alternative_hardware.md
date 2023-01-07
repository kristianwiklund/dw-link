An alternative hardware solution is described here: [AVR/Arduino Hardware Debugger on the Cheap](https://sites.google.com/site/wayneholder/debugwire3) which I (https://github.com/kristianwiklund/) happened to have built before I found this project

* RESET is connected to D10
* POWER is controlled by D9 - D9 high enables power to the device-under-test
* MOSI is connected to D11
* MISO is connected to D12
* SCK is connected to D13

Comparing this to the dw-link solution:

* MOSI - D11 (OK!)
* MISO - D12 (OK!)
* SCK - D13 (OK!)
* RESET - D8 (NOK - on Wayne's solution, this is an input to switch to programming mode)
* POWER - D9 (OK!)

We redefine the pin map to

    const PROGMEM pinmap  boardpm  = { 2, 5, 9, 7, 12, 10, 11, 15, 3, 6, 13, pundef } ;
    const byte SNSGND = 14;
    const byte DWLINE = 10; 

which is enabled by #defining ALTERNATE_BOARD_AHDOTC in the source code, and it should work.

