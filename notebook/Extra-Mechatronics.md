# Extra: setting up for Mechatronics

This guide assumes that you already followed the steps defined in [this document](WindowsSettings.md). In particular, MATLAB installation and configuration.

# Table of Contents
- [Extra: setting up for Mechatronics](#extra-setting-up-for-mechatronics)
- [Table of Contents](#table-of-contents)
- [Installing Software](#installing-software)
    - [Putty](#putty)
    - [HTerm](#hterm)
    - [STM32 CubeMX](#stm32-cubemx)
    - [STM32 System Workbench](#stm32-system-workbench)
- [Configuration](#configuration)
- [STM32F767ZI Nucleo PINs Cheatsheet](#stm32f767zi-nucleo-pins-cheatsheet)
- [Building a project using STM32 CubeMX](#building-a-project-using-stm32-cubemx)
    - [Project Settings](#project-settings)
        - [Stack Usage](#stack-usage)
    - [Assigning Purposes to PINs](#assigning-purposes-to-pins)
    - [GPIO Ports Configuration](#gpio-ports-configuration)
    - [Global Clock Configuration](#global-clock-configuration)
    - [USART Configuration](#usart-configuration)

# Installing Software

Make sure to install following programs (each element is a link to where to get them):
- [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
- [HTerm](http://www.der-hammer.info/terminal/hterm.zip)
- [STM32 CubeMX](https://www.st.com/en/development-tools/stm32cubemx.html#getsoftware-scroll)
- [STM32 System Workbench](http://www.openstm32.org/Downloading%2Bthe%2BSystem%2BWorkbench%2Bfor%2BSTM32%2Binstaller)

## Putty
Just install it.

## HTerm
- Extract `HTerm.exe` inside `"C:\Tools\"`
- Create a Desktop Shortcut
- Move the new shortcut to `"C:\Users\gabriele\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Tools"`

## STM32 CubeMX
Extract the zip file and execute the Windows Installer, accept everything as-is.

## STM32 System Workbench

Execute the installer, change installation path with `C:\Program Files\Ac6\SystemWorkbench` (for 64-bit version, for 32-bit `C:\Program Files (x86)\Ac6\SystemWorkbench`), then accpet everything else as-is.

> **NOTICE**: Installing System Workbench to a directory under `Program Files` (or its equivalent `Program Files (x86)`) leads to problems with permissions when using the program, since common users cannot write/modify content under subfolders of `Program Files`. Thus, either install it under another directory (example: `C:\Ac6\SystemWorkbench`) or change its permissions before opening the program.

# Configuration

**TODO**

# STM32F767ZI Nucleo PINs Cheatsheet

| User LEDs | Color | PIN          |
| --------- | ----- | ------------ |
| LED 1     | GREEN | PB0 (or PA5) |
| LED 2     | BLUE  | PB7          |
| LED 3     | RED   | PB14         |

| Service LEDs | Function             |
| ------------ | -------------------- |
| LED 4        | (TRIE color) ST-Link |
| LED 5        | USB Power Fault      |
| LED 6        | Power 5V             |
| LED 7/8      | USB OTG Status       |

| Button   | PIN           |
| -------- | ------------- |
| USER BTN | PC13 (or PA0) |
| RESET    | -             |

| Serial Ports | PIN |
| ------------ | --- |
| USART 3 TX   | PD8 |
| USART 3 RX   | PD9 |


# Building a project using STM32 CubeMX

This application uses the HAL libraries to generate code exactly for the stuff that we want to use, optimizing the overall code and letting us to concentrate only on the things that our controller will do, instead of what we need to configure via code to make this happen.

When first opening STM32 CubeMX (from now on simply Cube), click on `New Project`, then wait for the program to download all its stuff and then search for our target board, the `STM32F767ZI`. When you get the following screen, double-click it to create a new project.

![Selecting the board](mec-pics/01.png "Double click on the selected board")

## Project Settings

From the `Project` menu on top, let’s change its `Settings`. From this window we can choose destination folder of the project and generated code and which IDE we will use to write our application code. We will use STM32 System Workbench as IDE, thus we need to select `SW4STM32` as `Toolchain/IDE`. If prompted to download firmware for the board accept.

![Proj Conf Window](mec-pics/06.png "Project Configuration")

From the code generator tab we can customize some attributes about our project, like if we want that the libraries we use will be put as plain code in the project or simply linked in the makefile and so on. For now, we will leave all the standard settings in this tab.

Once settings are saved, we can use the configuration to generate code comprehending libraries and other configurations, depending on the other changes we made within this very project file. Once the code is generated, the `.project` file can be opened using STM32 System Workbench.


### Stack Usage
When programming a microcontroller, the size of the stack is really important. Since most of the code will be *interrupt-driven*, each call will consume stack to save the states of the registers to restore them later when the interrupt handler terminates.

The number of registers that will be saved may vary from call to call, but usually we have two cases:
- If the interrupt doesn't use any *floating-point arythmetic*, only 8 registers are pushed on the stack;
- Otherwise, 32 registers are pushed on the stack.

Since in general for multithreaded applications pretty much the whole code is interrupt-driven, we will consume a lot of stack for each call. This could lead to stack overlapping with the `.DATA` part of our program if we don’t select enough stack size in `Project Settings`.

Plus, we wouldn’t notice this problem until something very bad happens, like when we overwrite a global variable thinking to write in a local (stack) variable instead.




## Assigning Purposes to PINs
A PIN cannot be used if it is not set for a specific purpose (in/out, etc.). To assign a purpose to a PIN click on it and select the one that fits your requirements (consider looking at [these tables](#STM32F767ZI-Nucleo-PINs-Cheatsheet)).

![Pin Purpose](mec-pics/02.png "Single click to assign a purpose")

When a purpose is assigned you can even add a human-readable label to the PIN (one that can later be used within your code) if you right-click it and you select `Enter user label`.

![Pin Label](mec-pics/03.png "Right click and select `Enter user label`")

Go on and set up all following PINs:
| PIN  | Mode          | User Label  |
| ---- | ------------- | ----------- |
| PB0  | `GPIO_Output` | `LED_GREEN` |
| PB7  | `GPIO_Output` | `LED_BLUE`  |
| PB14 | `GPIO_Output` | `LED_RED`   |
| PC13 | `GPIO_Input`  | `BTN_USR`   |

## GPIO Ports Configuration
To configure GPIO Ports, switch to `Configuration` tab and select the `GPIO` entry.

![GPIO Conf](mec-pics/04.png "Select GPIO entry in Configuration tab")

From this panel, we can customize all ports configuration, the pull-up/down modes and so on. We will select an `open-drain/pull-up` mode for the `PB0` port (`LED_GREEN`). All the other ports will remain unchanged.

![GPIO Conf Window](mec-pics/05.png "GPIO PINS Configuration")


## Global Clock Configuration

Clock configuration can be achieved through different tools. The first one allows us to use *internal clock* as a base for our desired clock. In a new project, internal clock is selected as default, thus we just need to go under `Clock Configuration` tab if we want to tune the required variables.

![Internal Clock](mec-pics/07.png "Internal Clock Configuration")

For our purposes however the internal clock source is not enough.
We want to use the external oscillator present in our board to get a more reliable and precise clock (mostly to send data without errors through a serial line). The internal clock is not so reliable, since it is composed just by a simple Resistor-Capacitor circuit.

To switch to the *external clock* source, let's go back to `Pinout` tab and choose `RCC` entry in left panel, then select the `High Speed Clock (HSE)` by changing its value from `Disable` to `BYPASS Clock Source`.

![External Clock BYPASS](mec-pics/08.png "Set HSE to BYPASS Clock Source")

> **NOTICE**: The difference between the *high speed* clock and *low speed* one is just a matter of how fast you want to go. If your requirements don't need you to use the high speed clock you can stick with the low speed one for better power saving and lower consumptions. We'll stick with the high speed one however for simplicity.

Now let's go back to `Clock Configuration` again and select the correct values for the registers. Since the external source we have on our board (by the specifications) has a nominal frequency of 8 MHz, we will put this value in the `Input Frequency` block. Also, let's change the switch from `HSI` to `HSE`.

![Clock Frequency](mec-pics/09.png "Set 8MHz Input Frequency")

Also, select `PLLCLK` option in the switch at the right of the displayed one; this allows us to obtain a different clock frequency as a base for our final clocks (shown on the right-side of the scheme). The `PLLCLK` gives us many more configuration options than selecting only the `HSE` option (after which we can only set a prescaler).

Now let's look at the right side of the scheme and decide which values we want as resulting clocks. One of the clocks we are interested in is the `HCLK` one, which is used to communicate using the serial over USB cable. Since we want it as a multiple of 48 MHz, ~~even if I don't know why yet,~~ we can put 216 MHz in that block and let the program calculate the other values by itself. If we don't like final result we can modify them by hand later.

In particular, the program tells us when some configuration is in conflict with acceptable values from any component by flagging the wrong value as `red`. Adjust values until you get a suitable configuration (prescalers, `ABP1`, `ABP2`, and so on).
The following is a configuration which is compatible with previous requirements:

![Clock Registers Configuration](mec-pics/10.png "Set these values in this `Clock Configuration` tab")

> **NOTICE**: when generating code from Cube, there could be a bug in which the program will set the wrong `OscillatorType` in `RCC` configuration contained in generated code. Check that the generated code contains the following line exactly:
> `RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSE;`

## USART Configuration
*TODO*
