# blursed-embed
A collection of random knowledge and facts about embedded systems, that i find at least fascinating

## STM32
### IWDG and FLASH
 Source: https://community.st.com/t5/stm32-mcus-embedded-software/why-would-flash-flag-pgperr-and-flash-flag-pgserr-be-set-after-a/td-p/351906

> It was actually a bug introduced by me:

> I have a watchdog running and reset it frequently with
```
HAL_IWDG_Refresh(&hiwdg);
```
> For debugging purpose, I disable my watchdog init
```
// disable for debugging purpose
// MX_IWDG_Init();
```
> This causes problem in the Flash interface, where the FLASH_WaitForLastOperation() function fails.

Like how even the hell it works? As if you shut the toilet faucet, try to flush it, and now your towel cabinet won't open because of it!

### Debugger Power consumption

Quite suddenly, the debugger affects the power consumption of your board. And quite significantly, as a connected Stlink v3 can easily consume up to 350 μA. I won't investigate why exactly this happens but I suspect it has something to do with Vdd measurement. Yes, when you connect to MCU you usually see something like
```
Log output file:   C:\Users\Admin\AppData\Local\Temp\STM32CubeProgrammer_a02828.log
ST-LINK SN  : 0006002A4741500520383733
ST-LINK FW  : V3J15M7B5S1
Board       : STLINK-V3SET
Voltage     : 3.26V
SWD freq    : 8000 KHz
Connect mode: Under Reset
```
ADCs in STM32 have pretty low input impedance, so this can explain high current draw. But I didn't prove that, it's just a suggestion. So when dealing with low power applications be sure to measure your current draw WITHOUT debugger attached physically!

## ESP32
### FPU in ISR
Sources:
https://esp32.com/viewtopic.php?t=831
https://esp32.com/viewtopic.php?t=1292
https://www.reddit.com/r/esp32/comments/lj2nkx/just_discovered_that_you_cant_use_floats_in_isr/
Using floating-point arithmetic in ESP32 Interrupt Service Routines (ISRs) causes crashes (Coprocessor Exception) because the FPU state isn't saved by default. It’s not exactly a little-known fact, but it’s not explicitly mentioned anywhere! And unlike other popular microcontrollers, this is a surprising detail. Currently, there’s an experimental feature that allows you to use the FPU in ISRs, but it’s better to avoid doing so
