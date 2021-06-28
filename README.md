# PICO_PIC_colo
It is GUI Application Platform for RASPI PICO , has some debugging facilities.

GUI part(on windows PC) is written by PySimpleGUI and PICO part is written by C.
GUI part and PICO part communicate with USB_Serial.

So,you may start PICO programming with prepare the necessary bare minimum.(USB cable and jumper wire, of course PICO board! )
custom.py and custom.c are start pionts of your application.

More informations will take from PDF file.

-------------------------------------
Recent Changes
-------------------------------------
2021_0628
PICO_PIC_colo updates to beta 3.
This version can read/write large buffer upto 1024bytes.
And "Hide Main" button works well.
I hope this is final Beta :-p

2021_0624
PICO_PIC_colo updates to beta 2
Sorry, I find some bugs when checking.
 This version can't read/write large buffer( over 1024bytes?) with serial.
 "Hide Main" button can't hide some window. 

2021_0620
PICO_PIC_colo works on WindowsPC & RaspberryPi.

-------------------------------------
How to install PICO_PIC_colo on RASPI
-------------------------------------
Step1.
Unzip PICO_PIC_colo.zip at your project folder like as ~/pico/pico_project/PICO_PIC_colo

Step2.
Start PICO with uf2 file prepared

1) push bootsel sw
2) reset PICO
3) release bootsel sw
4) type as follows
'> cp PICO/build/PICO_PIC_colo.uf2 /media/pi/RPI-RP2'

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

1) Open VScodes for Python and PICO
2) Open source folder Python & PICO on VScode
3) Set tool chain Pyhon3.? and gcc_arm_none_eabi
4) Set pico-sdk path in CMakeList.txt about line 10
set(PICO_SDK_PATH "~/pico/pico-sdk")

Step6.
Clearn build at terminal on VScode for PICO

1) cd build
2) rm -r *
3) cmake ..
4) make -j4

Complete!

-------------------------------------
PICO_PIC_colo supports debugging
-------------------------------------
PICO_PIC_colo is a GUI application platform.
But, It is useful on debugging phase, even if you will develope stand alone application.

PICO_PIC_colo has check point function.
You can use check_point functions like below.

// call from main_init() @PICO_Pic_colo.c
void custom_init(void)
{
    init_ring_buff( &user_receive_ring_buff, user_receive_buff, BUFF_SIZE );
    init_ring_buff( &user_send_ring_buff, user_send_buff,BUFF_SIZE );

    set_cp_pass(0);  // Set PASS_MODE on CP_0
    set_cp_auto(1);  // Set AUTO_MODE on CP_1
    set_cp_auto(2);
    set_cp_auto(3);
    set_cp_auto(4);
    set_cp_hold(5);  // Set HOLD_MODE on CP_5
}

// call from main_loop() @PICO_Pic_colo.c
// so, call is stop when uart send/receive looping

void custom_main() {
    char c;
    check_point(0);  // It sets as PASS_MODE
    if( user_rec_ready_flag )
    {
        while( user_receive_ring_buff.flag == BUFF_NOT_EMPTY )
        {
            c = get_ring_buff( &user_receive_ring_buff );
            put_ring_buff( &user_send_ring_buff, c);
        }
        user_send_request_flag = true;
        user_rec_ready_flag = false;
        check_point(1); // It sets as AUTO_MODE
        check_point(2);
        check_point(3);
        check_point(4);
        check_point(5); // It sets as HOLD_MODE
    }
}

This is a sample to explain how to work check_points.

Ofcause, Your program is more complex, and you set check_point on several path.
If it is normal path, you may set check_point as PASS_MODE.
And if it is irregular path,you may set check_point as HOLD_MODE.

On PASS_MODE, check_point returns quickly,so it doesn't disturb program execution.
But, on HOLD_MODE, check_point executes infinit loop.
And it may break when you click Release button on Desktop.

So, you can find a cause to watch some variables. 
Watch variables from Desktop is another function which PICO_PIC_colo offer to support debugging.

If you develope applications without PICO_PIC_colo,
you can only use blinking LED for debugging.

It is too sad! isn't it?
