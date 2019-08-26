
# Building and Flashing MicroPython

 Overview:

- Use a Linux computer (or a virtual machine on a Windows host) to compile MicroPython and generate a hex file for flashing
- Use the ST-Link Utility to flash the hex file to a STM32 board on a Windows computer

You will need:

- Computer(s) with Linux and Windows installed
- STM32 Nucleo or STM32 Discovery microcontroller board
- [STM32 ST-Link Utility software](https://www.st.com/content/st_com/en/products/development-tools/software-development-tools/stm32-software-development-tools/stm32-utilities/stsw-link009.html)
- [Serial communication utility software such as PuTTY](https://putty.org/)
- MicroUSB cable
- A USB drive or SD card with some free storage

## Building MicroPython on Linux

1. In Linux or VirtualBox Debian Linux, install git, make, gcc, and gcc-arm-none-eabi by going to the command terminal and executing the following command

		sudo apt-get install git make gcc gcc-arm-none-eabi

2. Clone the MicroPython source repository by calling

		git clone https://github.com/micropython/micropython/

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
	In my case, I call

		make BOARD=B_L475E_IOT01A

	Upon success, the bottom of the message appears as:

		GEN build-{your-board-model-here}/firmware.dfu
		GEN build-{your-board-model-here}/firmware.hex

  
	If you have this message, you have successfully built MicroPython.

  

## Flashing Micropython to an STM32 Board on Windows

  

1. On the Linux Computer, find the firmware.hex file by calling the command

		find . -name firmware.hex 

in the home directory

2. Copy the firmware.hex file onto a USB drive or SD card.

3. Connect the board to the Windows computer through a microUSB cable. In addition, connect the USB drive to the Windows Computer.

4. Using STM32 ST-Link Utility software, select file>open and browse for firmware.hex file.

5. From the top bar, select Target>Connect.

6. From the top bar, select Target>Program & Verify. Leave default settings, click Start. If successful, you will see “Verification… OK” at the bottom status window. You can now close ST-Link Utility.

  

## Testing Micropython Flash

1. On a Windows Computer, Open Windows Device Manager.

2. Expand “Ports (COM & LPT). Look for the STLink Virtual COM Port entry and record the COM port. Once found, close device manager. In my case, my COM port to the STM32 board is COM3.

![STM32 COM Port on Deviec Manager.](https://lh3.googleusercontent.com/zMuWvsmk8PZ1kh2lc8s04SMKd4aKwScp8HWUv02gEPFtH66eVajKv7i0YTe-sJKFEsvANgqN47te "STM32 COM Port on Device Manager")

3. Open a serial communication utility software such as PuTTY.

4. Select “Serial” under Connection Type. Enter the COM port you recorded into the Serial Line field. Enter 115200 for speed (baudrate). Click Open.

![PuTTY Configuration for STM32 Board.](https://lh3.googleusercontent.com/rNBAGxMpglh9S_YgfcFAS5pPDw3CMrWAXjZ1Tb76lBhXtrdv9W6701I1iRQ44uDOtmo5SxeYJs1i)

5. Click inside the empty serial terminal window. Press the reset button on the board. A REPL display appears on the screen. In the REPL display, you can call the print function for python such as print(“Hello, World!”).

6. If the message you tried to print successfully shows up in the terminal, congratulations, you have successfully flashed micropython onto your microcontroller board.

7. To turn on an LED on the board, create an LED object and call the on function. If you want the LED off, call the off function.

		# Creating a LED object
		myled = pyb.LED(1)
		# Calling the on function
		myled.on()
		# Calling the off function
		myled.off()

   Or

	    pyb.LED(1).on()
	    pyb.LED(1).off()
