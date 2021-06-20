# PICO_PIC_colo
It is GUI Application Platform for RASPI PICO , has some debugging facilities.

GUI part(on windows PC) is written by PySimpleGUI and PICO part is written by C.
GUI part and PICO part communicate with USB_Serial.

So,you may start PICO programming with prepare the necessary bare minimum.(USB cable and jumper wire, of course PICO board! )
custom.py and custom.c are start pionts of your application.

More informations will take from PDF file.

2021_0620
PICO_PIC_colo works on WindowsPC & RaspberryPi.

-------------------------------------
How to install PICO_PIC_colo on RASPI
-------------------------------------
Step1.
Unzip PICO_PIC_colo.zip at your project folder like as ~/pico/pico_project/PICO_PIC_colo

Step2.
Start PICO with uf2 file prepared

1)push bootsel sw
2)reset PICO
3)release bootsel sw
4)type as follows
> cp PICO/build/PICO_PIC_colo.uf2 /media/pi/RPI-RP2

Then, PICO starts blinking!

Step3.
Set port name

in PICO_PIC_colo_comm.py
about line 18

def open():
    global uartport
    uartport = serial.Serial(
                port="/dev/ttyACM0",  <== USB serial

Step4.
run PICO_PIC_colo.py
push execute button on VScode for Python 

Step5.
Set build environment

1)Open VScodes for Python and PICO(C)
2)Open source folder PThon & PICO on VScode 
3)Set tool chain Pyhon3.? and gcc_arm_none_eabi
4)Set pico-sdk path in CMakeList.txt about line 10
# initalize pico_sdk from installed location
# (note this can come from environment, CMake cache etc)
set(PICO_SDK_PATH "~/pico/pico-sdk")
#set(PICO_SDK_PATH "C:/common/pico-project/pico-sdk")

Step6.
Clearn build at terminal on VScode for PICO

1) cd build
2) rm -r *
3) cmake ..
4) make -j4

Complete!
