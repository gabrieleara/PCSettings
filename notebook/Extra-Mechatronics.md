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
    - [Reading/Writing GPIO PINs](#readingwriting-gpio-pins)
    - [~~Reading~~/Writing on USART using Blocking API](#readingwriting-on-usart-using-blocking-api)
    - [Interrupts and USART3 Interrupt API](#interrupts-and-usart3-interrupt-api)
    - [Timers](#timers)
        - [Configurable Timers Characteristics](#configurable-timers-characteristics)
            - [Advanced-Control Timers (TIM1, TIM8)](#advanced-control-timers-tim1-tim8)
            - [General-Purpose Timers (TIMx)](#general-purpose-timers-timx)
            - [Basic Timers (TIM6, TIM7)](#basic-timers-tim6-tim7)
        - [Timer Clock Sources](#timer-clock-sources)
        - [Timer Modes for each Channel](#timer-modes-for-each-channel)
            - [Timer Input Capure Mode](#timer-input-capure-mode)
            - [Timer Output Compare Mode](#timer-output-compare-mode)
            - [Timer PWM Mode](#timer-pwm-mode)
            - [Timer One Pulse Mode](#timer-one-pulse-mode)
    - [Configuring a Timer (TODO)](#configuring-a-timer-todo)

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

Now let's look at the right side of the scheme and decide which values we want as resulting clocks. One of the clocks we are interested in is the `HCLK` one, which is used to communicate using the serial over USB cable.

> Clocks and timers within a Cortex processor are connected to various different clock sources, all delivered starting from the clock source provided on the left. These clock sources are:
> - `HCLK`: is the actual processor clock source and it drives SysTick timer.
> - `FCLK`: it is synchronized with HCLK, but it's not affected by sleeping condition of the processor, in which `HCLK` stops; `FCLK` ensures that interrupts can be sampled, and sleep events can be traced, while the processor is sleeping.
> - `APB1`/`APB2`: they stand for Advanced Peripheral Bus, on which there are the clock sources to which most peripherals and timers are connected. The relative clock sources are limited to a maximum of 54 and 108 MHz rispectively when driving *peripheral clocks* and 108 and 216 MHz rispectively when driving *timer clocks*. For a complete reference of which peripherals/clocks are connected to these two timers refer to the following table.


| Clock Source          | Maximum<br>Frequency<br>(default) | Timer/Peripheral                                                                                                                              |
| --------------------- | --------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| APB1 Peripheral Clock | 54 MHz                            | LPTIM1 WWDG SPI2/I2S2 SPI3/I2S3 SPDIFRX<br>USART2 USART3 UART4 UART5 UART7 UART8<br>I2C1 I2C2 I2C3 I2C4<br>CAN1 CAN2 CAN3<br>HDMI-CEC PWR DAC |
| APB1 Timer Clock      | 108 MHz                           | TIM2 TIM3 TIM4 TIM5 TIM6 TIM7 TIM12 TIM13 TIM14                                                                                               |
| APB1 Peripheral Clock | 108 MHz                           | USART1 USART6<br>ADC1 ADC2 ADC3<br>SDMMC1 SDMMC2<br>SPI1/I2S1 SPI4 SPI5 SPI6<br>SYSCFG SAI1 SAI2 DFSDM1<br>MDIO LTDC DSI                      |
| APB2 Timer Clock      | 216 MHz                           | TIM1 TIM8 TIM9 TIM10 TIM11                                                                                                                    |


Since we want the system clock to be as much high as possible, we will select 216 MHz in `HCLK` field; once that value is entered we can just let the program calculate the other values by itself. If we don't like final result we can modify them by hand later.

In particular, the program tells us when some configuration is in conflict with acceptable values from any component by flagging the wrong value as `red`. Adjust values until you get a suitable configuration (prescalers, `ABP1`, `ABP2`, and so on).
The following is a configuration which is compatible with previous requirements:

![Clock Registers Configuration](mec-pics/10.png "Set these values in this `Clock Configuration` tab")

> **NOTICE**: when generating code from Cube, there could be a bug in which the program will set the wrong `OscillatorType` in `RCC` configuration contained in generated code. Check that the generated code contains the following line exactly:
> `RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSE;`

## USART Configuration

For our purposes, we will use the USART3 functionality to perform serial communication over USB. To configure this, select the `Pinout` tab, under the `USART3` voice in left panel set the mode to `Asynchronous`. Then let's assign PINs to `USART3`. Set `PD8` as `USART3_TX` and `PD9` as `USART3_RX`.

![USART Mode](mec-pics/11.png "Set the mode to `Asynchronous`")
![USART PINs](mec-pics/12.png "Set the two PINS as Tx/Rx")

Next move to `Configuration` tab and click on `USART3` to configure the USART with the following settings:

| Parameter         | Value                             |
| ----------------- | --------------------------------- |
| Baud Rate         | 115200 Bits/s (automatically set) |
| Word Length       | 8 Bits (including parity)         |
| Parity            | None                              |
| Stop Bits         | 1                                 |
| Data Direction    | Receive and Transmit              |
| Over Sampling     | 16 samples                        |
| Single Sample     | Disable                           |
| Advanced features | All Disabled                      |

> **NOTICE**: For low speed ports it’s better to have 16 samples per transmit, but at higher speeds it won’t be possible to configure it like this, due to clock limitations.

![USART Configuration](mec-pics/13.png "Set the values as in previous table")

> The timeout for the transmit operation has to be big enough so that for each bit that has to be transmitted the program will wait for at least one cent of millisecond; considering that the transmit requires 10 bits per byte, the global wait time must be at least one tenth of millisecond per byte.

Once code is generated we can modify it (in the appropriate spaces) to send data through USART3 and we can read data using PuTTY on PC for example. 

Now if you have a serial emulator terminal like Putty you can read the transmitted value.

## Reading/Writing GPIO PINs

If we want to check wether the PINs we set work, we can modify generated code so that a LED will light up when we press the user button. To do so, let's go in `main` function and insert the following lines within the infinite loop:
``` c
if (HAL_GPIO_ReadPin(BTN_USR_GPIO_Port, BTN_USR_Pin))
{
    HAL_GPIO_WritePin(LED_BLUE_GPIO_Port, LED_BLUE_Pin, 1);
}
else
{
    HAL_GPIO_WritePin(LED_BLUE_GPIO_Port, LED_BLUE_Pin, 0);
}
```

Or in a more compact way:
``` c
HAL_GPIO_WritePin(LED_BLUE_GPIO_Port, LED_BLUE_Pin,
    HAL_GPIO_ReadPin(BTN_USR_GPIO_Port, BTN_USR_Pin));
```

## ~~Reading~~/Writing on USART using Blocking API

Since we haven't configured any interrupt/DMA mechanism yet, we can only send data on USART synchronously. To do so, the following code can be used (again, put it within infinite while loop):
```c
HAL_UART_Transmit(&huart3, "HelloWorld\r\n", 12, 4);
```
where:
- `&huart3` is the address of our USART handler;
- `"HelloWorld\n"` is the pointer to the message we want to send;
- `11` is the length of the message;
- `4` is the timeout, expressed in ~~milliseconds~~ **_???_**.

If we now start PuTTY on Windows and we select the correct port with the settings we put in our USART3 configuration we can read the values. The right port name can be found if we open `Device Manager` on Windows, under `Serial Ports (COM and LPT)`.

## Interrupts and USART3 Interrupt API

In the way we programmed until now, we cannot create parallel tasks (in a timed or in any way in general). To do so, in particular for time-activated tasks, we need to configure the interrupt environment. When enabled, a peripheral can be also controlled using interrupts, instead of setting a timeout to wait for the blocking operations to complete like we did before.

The first action that we need to do is to enable interrupts in Cube. In the `Configuration` tab, select `NVIC` (Nested Vector Interrupt Controller) entry to open interrupt handler configuration. From this window we can change priorities of interrupts and other configurations.

The highest priority in the ARM architecture is the 0-priority, while the lowest is 15 (if 4 bits of *preemption priority*, also called *sub-priority* are used). Higher priority interrupts can preempt only strictly lower priority interrupt handlers (this is true also if we enable also sub-priorities).

The sub-priority is used only if the processor has to choose between two or more same-priority interrupts in a given moment. Typically this happens whenever a high-priority interrupt ends its execution and the processor has to choose between more than one interrupt with same priority. This is the only case where sub priority has a meaning, and once again, lower absolute value means higher sub-priority.

> While usually the 0-priority is reserved to faults or other system interrupts, we can use it because their trigger is so unlikely that when it happens it's something so bad that our system could be irreversiverly damaged, like when the processor encounters very big problems. However the good practice is to change the priority of peripheral handlers to a non-zero value.

![NVIC Configuration](mec-pics/14.png "NVIC Configuration window")

In alternative, each interrupt handler can be enabled/managed from the configuration window of the associated device. In our case the `USART3` controller. For the USART3 interrupt we will configure the lowest priority possible (without sub priorities) which is 15.

> **Notice**: if we have a system where tasks are activated at different rates using different timers, assigning to these timers different priorities will configure also a priority between their associated tasks.

![USART3 NVIC Configuration](mec-pics/15.png "Usart3 NVIC Configuration tab")

![USART3 NVIC Priority](mec-pics/16.png "Set USART3 Priority to 15 (lowest priority)")

Now that we have configured the USART3 to use interrupts we can use its Interrupt API calls to send data asynchronously. We can even override `__weak` functions defined by Cube to react when transmission is finished/data is received. `__weak` functions defined by Cube do nothing.

Of course, since calls for transmitting are not blocking anymore we cannot use them within the infinite loop. We can however write the first transmit just before the infinite loop and then define the handler for when the transmit is completed, like this:
``` c
/* USER CODE BEGIN PV */
/* Private variables ---------------------------------------------------------*/
static char text[] = "HelloWorldIT\r\n";
/* USER CODE END PV */

/* ... */

/* USER CODE BEGIN 0 */
void HAL_UART_TxCpltCallback(UART_HandleTypeDef *huart)
{
    /* Interrupt-based transmit has no timeout,
     * since it's not blocking.
     */
    HAL_UART_Transmit_IT(&huart3, (unsigned char*) text, strlen(text));
}
/* USER CODE END 0 */

/* ... */

int main(void)
{
    /* ... */

    /* USER CODE BEGIN 2 */
    HAL_UART_Transmit_IT(&huart3, (unsigned char*) text, strlen(text));
    /* USER CODE END 2 */

    while(1)
    {
        /* ... */
    }
}
```
## Timers

Now we can execute code triggered by interrupts, however usually we want to execute code at fixed intervals of time, thus a timer is needed.

> **NOTICE**: It’s never a good idea to use the system timer inside the ARM processor (SysTick) to schedule tasks. Our systems will be developed on bare-metal microprocessors, but usually that timer is reserved to Operating Systems to do their stuff. If we'd like our code to be portable, we should design our modules using other timers. SysTick timer has a fixed 0-priority and cannot be changed.

The board we use includes two advanced-control timers, eight general-purpose timers, two basic timers and two watchdog timers. All timers have a 16-bit prescaler. The following table is based on the one included in the datasheet:

| Timer<br>Type        | Timer          | Clock Max<br>Frequency<br>(MHz) | Counter<br>Resolution | Counter<br>Type       | DMA<br>Request<br>Generation | Capture/<br>Compare<br>Channels |
| -------------------- | :------------: | :-----------------------------: | --------------------- | --------------------- | :--------------------------: | :-----------------------------: |
| Advanced-<br>Control | TIM1<br>TIM8   | 216                             | 16-bit                | Up<br>Down<br>Up/Down | Yes                          | 4                               |
| General<br>Purpose   | TIM2<br>TIM5   | 108                             | 32-bit                | Up<br>Down<br>Up/Down | Yes                          | 4                               |
| General<br>Purpose   | TIM3<br>TIM4   | 108                             | 16-bit                | Up<br>Down<br>Up/Down | Yes                          | 4                               |
| General<br>Purpose   | TIM9           | 216                             | 16-bit                | Up                    | No                           | 2                               |
| General<br>Purpose   | TIM10<br>TIM11 | 216                             | 16-bit                | Up                    | No                           | 1                               |
| General<br>Purpose   | TIM12          | 108                             | 16-bit                | Up                    | No                           | 2                               |
| General<br>Purpose   | TIM13<br>TIM14 | 108                             | 16-bit                | Up                    | No                           | 1                               |
| Basic                | TIM6<br>TIM7   | 108                             | 16-bit                | Up                    | Yes                          | 0                               |

### Configurable Timers Characteristics

#### Advanced-Control Timers (TIM1, TIM8)
The advanced-control timers (TIM1, TIM8) can be seen as three-phase PWM generators multiplexed on 6 channels. They have complementary PWM outputs with programmable inserted dead times. They can also be considered as complete general-purpose timers. Their 4 independent channels can be used for:
- Input capture
- Output compare
- PWM generation (edge- or center-aligned modes)
- One-pulse mode output

If configured as standard 16-bit timers, they have the same features as the general-purpose TIMx timers. If configured as 16-bit PWM generators, they have full modulation capability (0-100%).
They support independent DMA request generation. 


#### General-Purpose Timers (TIMx)
There are ten synchronizable general-purpose timers embedded in the STM32F76xxx devices.
- Full-featured general-purpose timers (TIM2, TIM3, TIM4, TIM5)
    - The TIM2 and TIM5 timers are based on a 32-bit auto-reload up/downcounter.
    - The TIM3 and TIM4 timers are based on a 16-bit auto-reload up/downcounter.
    - They all feature 4 independent channels for input capture/output compare, PWM or one-pulse mode output. This gives
up to 16 input capture/output compare/PWMs ~~on the largest packages~~.
    - They all have independent DMA request generation and they are capable of handling quadrature (incremental) encoder signals and the digital outputs from 1 to 4 hall-effect sensors.
- Limited general-purpose timers (TIM9, TIM10, TIM11, TIM12, TIM13, TIM14)
    - These timers are based on a 16-bit auto-reload upcounter.
    - TIM10, TIM11, TIM13, and TIM14 feature one independent channel, whereas TIM9 and TIM12 have two independent channels for input capture/output compare, PWM or one-pulse mode output. They can also be used as simple time bases.

#### Basic Timers (TIM6, TIM7)
These timers are mainly used for DAC trigger and waveform generation. They can also be used as a generic 16-bit time base.
TIM6 and TIM7 support independent DMA request generation.

### Timer Clock Sources
The timer always needs a clock source. It also can be synchronized by several clocks
simultaneously:
- Internal clock, clock is the default provided by RCC; its frequency depends wether the timer belongs to APB1 or APB2 buses.
- External clock, in this cases the clock is provided by an external signal connected to TIx pins or ETR pin. The maximum external clock frequency should be verified.

### Timer Modes for each Channel

Each timer which supports one or more channels supports different modes in which these channels can be used, each with a different purpose.

#### Timer Input Capure Mode

The timer can be used in input capture mode to measure an external signal. Depending on timer clock, prescaler and timer resolution, the maximum measured period is deduced.

The input capture module is used to capture the value of the counter after a transition is detected by the corresponding input channel. To get the external signal period, two consecutive captures are needed. The polarity index is 1 if the rising or falling edge is used, and 2 if both edges are used.

#### Timer Output Compare Mode

To control an output waveform, or to indicate when a period of time has elapsed, the value of the counter `CNT` is compared with respect to the content of a compare register `CRRx`, where `x` is the index of the channel.

Usually a signal can be set high when the value in `CNT` is either exactly equal to~~, greater or lower than~~ the content of `CRRx`, depending on the mode. This can be used to generate multiple timed events for each period of the timer (one for each channel).

#### Timer PWM Mode

The timer is able to generate PWM in edge-aligned mode or in center-aligned mode with a frequency determined by the value of the `TIMx_ARR` register, and a duty cycle determined by the value of the `TIMx_CCRy` register, where `y` is the associated channel.

There are two modes, the first one:
- PWM mode 1
    - In up-counting, channel`y` is *active* as long as `CNT < CCRy`, otherwise it is inactive.
    - In down-counting, channel`y` is *inactive* as long as `CNT > CCRy`, otherwise it is active.
- PWM mode 2
    - In up-counting, channel`y` is *inactive* as long as `CNT < CCRy`, otherwise it is active.
    - In down-counting, channel`y` is *active* as long as `CNT > CCRy`, otherwise it is inactive.

When we combine these modes with the different counting modes that we can select, we obtain the following scenarios (which are also resumed in the following table):
- if we adopt PWM mode 1 and `CNT` is reset to zero when `ARR` value is reached, then we get a *left/right-aligned PWM signal with duty cycle $\frac{CCR}{ARR}$*, where alignment depends on the counting direction, rispectively up/down.
- if we adopt PWM mode 1 and counting mode is inverted when `CNT` reaches `ARR` or zero value, then we get a *middle-aligned PWM signal with duty cycle $\frac{CCR}{ARR}$*.
- if we adopt PWM mode 2 and `CNT` is reset to zero when `ARR` value is reached, then we get a *right/left-aligned PWM signal with duty cycle $\frac{ARR-CCR}{ARR}=1-\frac{CRR}{ARR}$*, where alignment depends on the counting direction, rispectively up/down.
- if we adopt PWM mode 2 and counting mode is inverted when `CNT` reaches `ARR` or zero value, then we get a *middle-aligned PWM signal with duty cycle $\frac{ARR-CCR}{ARR}=1-\frac{CRR}{ARR}$*.

| PWM Mode | Counting Mode | Duty Cycle          | Alignment |
| :------: | ------------- | :-----------------: | :-------: |
| 1        | Up            | $\frac{CCR}{ARR}$   | left      |
| 1        | Down          | $\frac{CCR}{ARR}$   | right     |
| 1        | Up/Down       | $\frac{CCR}{ARR}$   | middle    |
| 2        | Up            | 1-$\frac{CCR}{ARR}$ | right     |
| 2        | Down          | 1-$\frac{CCR}{ARR}$ | left      |
| 2        | Up/Down       | 1-$\frac{CCR}{ARR}$ | middle    |

#### Timer One Pulse Mode

If needed, refer to [STM32 cross-series timer overview](https://www.st.com/content/ccc/resource/technical/document/application_note/54/0f/67/eb/47/34/45/40/DM00042534.pdf/files/DM00042534.pdf/jcr:content/translations/en.DM00042534.pdf) (also known as *Application Note AN4013*), section 2.6.

## Configuring a Timer (TODO)

To do so, we need to enable a timer from the left panel of `Pinout` tab.

Following is the configuration needed to enable `TIM1` timer:

| Attribute     | Value                     |
| ------------- | ------------------------- |
| Clock Source  | Internal Clock            |
| Channel1      | Input Capture direct mode |
| Channel2      | Output Compare CH2        |
| Anything else | Disabled                  |

![TIM1 Enable](mec-pics/17.png "Enable TIM1 timer using these configurations")





Once the timer is enabled we can change its settings under `Configuration` tab.

