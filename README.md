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