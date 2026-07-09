ad9288 can be replaced with ad9218 for higher resolution


the AFE is designed for 1GSPS so 100MSPS should work just fine

the STM32H523RETx can be replaced with STM32H562RGTx for higher performance

also you NEED to implement the microcontroller IC in the PCB because you need to make sure the all 8/10 of the ADC's output signals arrive at the same time

ad9288/9218 have different sampling rate options - but this requires changing out the CMOS oscillator accordinly as well. the current option is for a low jitter 100MHz clk signal: ASEM1-100.000MHZ-LC-T 