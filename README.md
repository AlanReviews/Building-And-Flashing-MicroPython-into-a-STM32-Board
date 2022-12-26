
# Building and Flashing MicroPython

 Overview:

- Use a Linux computer (or a virtual machine on a Windows host) to compile MicroPython and generate a hex file for flashing
- Use the STM32CubeProgrammer to flash the hex file to an STM32 board on a Linux/Mac/Windows computer

You will need:

- Computer(s) with Linux installed, e.g. Ubuntu
- STM32 Nucleo or STM32 Discovery microcontroller board
- [STM32CubeProgrammer](https://www.st.com/en/development-tools/stm32cubeprog.html)
- [Serial communication utility software such as PuTTY](https://putty.org/)
- MicroUSB cable

## Building MicroPython on Linux

1. In Linux or VirtualBox Debian Linux, install git, make, gcc, and gcc-arm-none-eabi by going to the command terminal and executing the following command

		sudo apt-get install git make gcc gcc-arm-none-eabi

2. Clone the MicroPython source repository by calling

		git clone https://github.com/micropython/micropython/
        
    Alternatively, with the GitHub CLI, call

        gh repo clone micropython/micropython

3. Go to the MicroPython directory by calling

		cd micropython

4. Compile mpy-cross by going to the mpy-cross directory under micropython and call make

		cd mpy-cross
		make

5. Leave the mpy-cross directory by calling

		cd ..

6. Go to the port directory and update STM32 submodules by calling

		cd ports/stm32
        make submodules

7. Build the STM32 firemoware for a specific board by callihng

		make BOARD={your-board-model-here}

	In my case, I use

		make BOARD=B_L475E_IOT01A

	Upon success, the bottom of the message appears as:

		GEN build-B_L475E_IOT01A/firmware.bin
        GEN build-B_L475E_IOT01A/firmware.dfu
		GEN build-B_L475E_IOT01A/firmware.hex

  
	If you have a similar or exact message, you have successfully built MicroPython.

  

## Flashing Micropython to an STM32 Board

  

1. On the Linux Computer, find the firmware.hex file by calling the command

		find . -name firmware.hex 

	under the micropython directory

2. Download and install the STM32CubeProgrammer software on either Linux, Mac, or Windows.

3. Connect the board to the computer through a microUSB cable.

![](/images/stm32cubeprogrammer%20startup.png))

4. In the STM32CubeProgrammer, press Connect button near the top right corner.

![](/images/stm32cubeprogrammer%20connected.png))

5. Under Erase and programming, browse for firmware.hex file.

![](/images/stm32cubeprogrammer%20in%20erasing%20and%20programming.png)

6. Flash the file by pressing the Start Programming button.

As a result, here are the logs
```
10:44:40 : File download complete
10:44:40 : Time elapsed during download operation: 00:00:09.719
10:44:40 : Verifying ...
10:44:40 : Read progress:
10:44:42 : Download verified successfully
10:44:42 : RUNNING Program ...
10:44:42 :   Address:  	: 0x08000000
10:44:42 : Application is running, Please Hold on...
```


## Testing Micropython Flash

1. On a Windows Computer, open Windows Device Manager.

2. Expand “Ports (COM & LPT)". Look for the STLink Virtual COM Port entry and record the COM port. Once found, close device manager. In my case, my COM port to the STM32 board is COM3.

![STM32 COM Port on Device Manager.](/images/list%20of%20ports.png)

3. Open a serial communication utility software such as PuTTY.

4. Select “Serial” under Connection Type. Enter the COM port you recorded into the Serial Line field. Enter 115200 for speed (baudrate). Click Open.

![PuTTY Configuration for STM32 Board.](/images/PuTTY%20Configuration.png)

5. Click inside the empty serial terminal window. Press the reset button on the board. A REPL display appears on the screen. In the REPL display, you can call the print function for Python such as

		print(“Hello, World!”)

6. If the message you tried to print successfully shows up in the terminal, congratulations, you have successfully flashed micropython onto your microcontroller board.

7. To turn on an LED on the board, create an LED object and call the on function. If you want the LED off, call the off function.

		# Create a LED object
		myled = pyb.LED(1)
		# Call the on function to turn on the LED
		myled.on()
		# Call the off function to turn off the LED
		myled.off()


# Resources
- [MicroPython GitHub page](https://github.com/micropython/micropython)
- [Python Discord](https://discord.gg/python)
