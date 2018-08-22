# Extra: setting up for Mechatronics

This guide assumes that you already followed the steps defined in [this document](./WindowsSettings.md). In particular, MATLAB installation and configuration.

# Installing Software

Make sure to install following programs (each element is a link to where to get them):
- [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
- [HTerm](http://www.der-hammer.info/terminal/hterm.zip)
- [STM32 CubeMX](https://www.st.com/en/development-tools/stm32cubemx.html#getsoftware-scroll)
- [STM32 System Workbench](http://www.openstm32.org/Downloading%2Bthe%2BSystem%2BWorkbench%2Bfor%2BSTM32%2Binstaller)

## Putty
Just install it.

## HTerm
- Extract `Hterm.exe` inside `"C:\Tools\"`
- Create a Desktop Shortcut
- Move the new shortcut to `"C:\Users\gabriele\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Tools"`

## STM32 CubeMX
Extract the zip file and execute the Windows Installer, accept everything as-is.

## STM32 System Workbench
Execute the installer, change installation path with `C:\Program Files\Ac6\SystemWorkbench` (for 64-bit version, for 32-bit `C:\Program Files (x86)\Ac6\SystemWorkbench`), then accpet everything else as-is.

# Configuration
