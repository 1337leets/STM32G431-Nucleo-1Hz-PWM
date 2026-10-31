# STM32G431 Nucleo 1 Hz PWM

A minimal STM32CubeIDE project for the NUCLEO-G431RB board. It outputs a 1 Hz square wave with a 50 % duty cycle on PA5 using hardware PWM, with nothing to do in the main loop. It is a small starting point for beginners and hobbyists who need a slow clock signal.

On this board, PA5 also drives the green user LED (LD2), so the LED blinks once per second.

## How the Frequency Is Set

The system clock runs at 170 MHz, from the internal 16 MHz oscillator through the PLL. TIM2 channel 1 divides it down:

```
f = 170 MHz / (Prescaler + 1) / (Period + 1)
  = 170 MHz / 17000 / 10000
  = 1 Hz
```

| Setting | Value | Effect |
|---------|-------|--------|
| `Prescaler` | 16999 | Timer counts at 10 kHz |
| `Period` | 9999 | One cycle lasts 1 s |
| `Pulse` | 5000 | Output is high for half of the cycle |

To get another frequency, change `Period` and `Pulse` in `ClockSignal.ioc` or in `MX_TIM2_Init` in `Core/Src/main.c`. For example, a `Period` of 999 and a `Pulse` of 500 give 10 Hz.

## Using It

1. Open STM32CubeIDE and choose File, Import, Existing Projects into Workspace.
2. Select this folder.
3. Connect the board, then build and run the project.

The output is on PA5, which is pin D13 on the Arduino header. It is a 3.3 V signal, so do not drive a load directly from it.

## License

MIT for the project code written here. The files under `Drivers/` and the generated files are from STMicroelectronics and keep their own license terms.
