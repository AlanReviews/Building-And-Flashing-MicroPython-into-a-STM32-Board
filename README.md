
# Building and Flashing MicroPython

 Overview:

- Use a Linux computer (or a virtual machine on a Windows host) to compile MicroPython and generate a hex file for flashing
- Use the ST-Link Utility to flash the hex file to a STM32 board on a Windows computer

You will need:

- Computer(s) with Linux and Windows installed
- STM32 Nucleo or STM32 Discovery microcontroller board
- STM32 ST-Link Utility software
- Serial communication utility software such as PuTTY
- MicroUSB cable
- A USB drive or SD card with some free storage

## Building MicroPython on Linux

1. In Linux or VirtualBox Debian Linux, install git, make, gcc, and gcc-arm-none-eabi by going to the command terminal and executing the following command

		sudo apt-get install git make gcc gcc-arm-none-eabi

2. Clone the MicroPython source repository by calling

		git clone <https://github.com/micropython/micropython/>

3. Go to the MicroPython directory by calling

		cd micropython

4. Call

		git submodule update --init

6. Compile mpy-cross by going to the mpy-cross directory under micropython and call make

		cd mpy-cross
		make

7. Leave the mpy-cross directory by calling

		cd ..

8. Go to the port directory by calling

		cd ports/stm32

9. Call

make BOARD={your-board-model-here}

9. In my case, I call

		make BOARD=B_L475E_IOT01A

Upon success, the bottom of the message appears as:

	GEN build-{your-board-model-here}/firmware.dfu
	GEN build-{your-board-model-here}/firmware.hex

  

If you have this message, you have successfully built MicroPython.

  

## Flashing Micropython to an STM32 Board on Windows

  

1. On the Linux Computer, find the firmware.hex file by calling the command

find . -name firmware.hex in the home directory

2. Copy the firmware.hex file onto a USB drive or SD card.

3. Connect the board to the Windows computer through a microUSB cable. In addition, connect the USB drive to the Windows Computer.

4. Using STM32 ST-Link Utility software, select file>open and browse for firmware.hex file.

5. From the top bar, select Target>Connect.

6. From the top bar, select Target>Program & Verify. Leave default settings, click Start. If successful, you will see “Verification… OK” at the bottom status window. You can now close ST-Link Utility.

  

## Testing Micropython Flash

1. On a Windows Computer, Open Windows Device Manager.

2. Expand “Ports (COM & LPT). Look for the STLink Virtual COM Port entry and record the COM port. Once found, close device manager. In my case, my COM port to the STM32 board is COM3.

  ![enter image description here](https://lh3.googleusercontent.com/B_fB32QRwh01Hfr-m9gmQTieWBxNDnc_okpm19mrswF8_gaIHINLUZhTHUuPPFNj4PXsLAhC0ixW "Finding the COM Port in Device Manager")

3. Open a serial communication utility software such as PuTTY.

4. Select “Serial” under Connection Type. Enter the COM port you recorded into the Serial Line field. Enter 115200 for speed (baudrate). Click Open.

  ![enter image description here](https://lh3.googleusercontent.com/LNZD_LdqCcaPcMuu9ltVkdxw-DPulNIyxluslvOgMNxdn1p20Uv4EBxK5fyQoiwc9n450z6dYh1v "PuTTY Configuration for STM32 Board")


5. Click inside the empty serial terminal window. Press the reset button on the board. A REPL display appears on the screen. In the REPL display, you can call the print function for python such as print(“Hello, World!”).

6. If the message you tried to print successfully shows up in the terminal, congratulations, you have successfully flashed micropython onto your microcontroller board.

7. To turn on an LED on the board, create an LED object and call the on function. If you want the LED off, call the off function.

		Creating a LED object
		myled = pyb.LED(1)
		Calling the on function
		myled.on()
		Calling the off function
		myled.off()

   Or

	    pyb.LED(1).on()
	    pyb.LED(1).off()
