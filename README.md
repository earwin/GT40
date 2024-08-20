	A simulation of the GT40. A graphics terminal manufactured by the Digital Equipment Corporation (DEC).
The simulation is based upon the pdp11 simulator from Simh. https://github.com/simh/simh
The original display system code developed by Matt Burke, Phil Budne, Alan Frisbie and others
has been significantly modified to add a simulated KY11-J front panel which is functional.
Similarly, I would like to thank Lars Brinkhoff for his expert assistance is getting the ITS
graphics interfaces running.
The original graphics for this panel were provided by Joerg Hoppe and are planned to be included
in his Blinkebone-panelsim system at some stage. The rationale behind this project is that
we can now move beyond simple terminal windows for these simulated system to simulations that
allow for a more coherent experience of using these old computers. Clearly this is why Oscar's
PiDP8 and PiDP11 have been so successful as have Joerg's simulated panels which give us much
more of a feel for how these systems were used. In addition, the GT40 was one of the graphics
systems that were developed at the same time as the PDP10 and PDP11. I would note the PDP10 in
particular as Oscar is in the process of developing the PiDP10 and, to run on this system, ITS
is probably the main game in town for which there is also graphics support.

Introduction:

 The GT40 is a self contained graphics/text terminal developed by DEC and first introduced
in 1972. A complete outline of this device can be found in the users guide:
EK-GT40-OP-002_GT40_GT42_Users_Guide_Feb75.pdf This is included in this download.

Building the system:

The download contains makefiles for Linux, MacOSX and Windows.

This application depends upon SDL2 for graphics. Please install the dev files
for this first.

eg (Linux) sudo apt-get install libsdl2-dev
   (MacOSX) brew install sdl2
   (Windows) for this OS you will need the windows build system for simh. This is contained
   in a separate directory and should be included in your VS projects directory. 

1.	Linux
	From GT40 directory, make will build the gt40 binary and make a copy
	in the BIN Directory.
2.	MacOSX
	From the GT40 directory, build with make -f makefile.simh. This will result in a binary
	name 'pdp11' in the BIN directory. You can rename it to gt40. The reason for not changing
	the name of the executable is due to the complex structure of the simh makefile.
3.	Windows
	There is a Visual Studio project file in the 'Visual Studio Projects' directory. To build
	this you will need a version of Visual Studio >= 2008.


Howto:

	The GT40 is a terminal that is connected to a master system via a serial line. It consists
of a pdp 11/05 processor with a VT11 display system. To act as a terminal the 11/05 requires a
programme. First, try the script rt11 in the .BIN directory with
(Linux/OSX) ./gt40 rt11 <your master system name>:<port number>
This script will setup a DL11 serial port and connect it to the master system and port as above.
Also, the script will attach an RT11 image to RL0 and a graphics utilities disk to RK0.
In addition, the script will load the standard GT40 rom.
Finally, the script will load a terminal interface app (vt07.bin) that maybe used to connect to ITS.
To this end, there are a number of options:

1. 	The GT40 was supplied with a boot rom containing a minimal terminal interface programme.
	An image of the rom is in ./BIN/bootvt.img and this can be loaded in the simh rom system.
	The rom image is loaded to address 17766000 which is also the start address.
	This may be started by pressing 'Load Address' and the 'Start' as the SR is already set.
	Apparently, nothing will happen. This is because, being a serial terminal, any characters
	sent before starting the programme will be lost.
	If you test it using the telnet port to another system, just start typing the username
	and the login prompt will appear. The, you are online with a very basic terminal. It is
	little better than a teletype.

2.	If you wish, you can use the VT07 terminal system to connect to any system, not just ITS.
	Set the SR to 1000 press 'Load Address' and 'Start' and carry on as above. This is a slighly more
	reliable terminal which also contains a graphics interface and may be used with EMACS under ITS.
	Again, you will see only a cursor. Start typing to move things along.

3.	RT11 may be used to build graphics apps with either BASIC/RT11 saved as 
	GBASIC.SAV or, FORTRAN. See DRAW.FOR. RT11 can be booted from the simh prompt with boot rl0.
	In addition, there is an RT11 app GTERM.SAV. This is another very simple terminal programme
	that uses the RT11 VT11 scroller system to display the text. It is not a VT100 sim!!!!!
	Alternatively you can be a real programmer and toggle in the RL02 bootstrap from the 11/05
	panel.
	See: DEC-11-LBACA-D-D_basicRTlangRef_Oct74 (included)

4.	If you want to try your hand with ITS, you will need to download the latest build from github
	https://github.com/PDP-10/its. To connect to this, you can use either the VT07 script as above
	to connect to port 10019 on the ITS system. Or, use the rom bootstrap, login to ITS and then the
	GTLOAD app to load the VT07 binary from the ITS server. Then you can play with Lisp/Logo
	or the incomprehensible Stanford University Drawing System (SUDS)!
	
5.	The focal script loads and runs FocalGT. This also contains a primitive terminal
	interface that is usually used with VTCOM under RT11.
	See: FOCAL-GT_RT_UsersMan

6.	Finally, games. Needless to say, Lunar Lander is still in and can be run from the rt11 script
	simh prompt with boot vt. There is a copy of Space War that can be loaded with the command
	load sw.lda and then run. This game uses the switch register for control. Find out which
	one is which yourself!
	NB: to boot vt for Lunar lander use this script:<br>
	set vt crt=vr14<br>
	set vt scale=2<br>
	set vt ena<br>
	boot vt<br>

	In conclusion, this GT40 app is another step towards a complete virtual machine and at least
	provides a genuine green screen experience. I think the display resolution is a bit low at
	the moment. However, scaling the whole thing is a bit of a challenge as it already takes up
	quite a bit of space on my 2560x1440 (1440P) monitor. For the next step, a 4K monitor is
	required!
	Also, I would request that if anyone has a really nice picture of a VR17 that could be used
	instead of the current VR14, do drop me a line.

	This software is provided under GPLv3.
	Ian Schofield Jan 2021.

	Comments for 2022::
	
1.	This app will not buid for Windows using Visual Studio 2019/2022 due to the well known __iob_func problem.
	This error is quite hard to fix and requires a new version of SDL. I stongly suggest that you use Visual
	Studio 2008 at the moment until this can be fixed. In the meanwhile, change the linked library setting to
	include SDL2.dll and remove the definition HAVE_LIBPNG.<br>
	
2.	There is a new version of Spacewar available. Many thanks to Lars Brinkhoff for sorting this out.<br>
	It can be found at: https://github.com/pdp11/mit-gt40-spacewar<br>
	This also means that you can become a DOS-11 expert!
<br>
	Update Oct 2023:<br>
 <br>
 	This update is for evaluation purposes at the moment.<br> 1: recent 64 bit SDL2 libraries do not allow
	UpdateWindowSurface off the SDL main thread. Mloop() and refresh() have been updated to correct this.
	This does affect the screen refresh so any comments about display quality would be great.<br>
	2: console command injection which is used for key strokes into the GT40 UI window has been updated
	as the use of TIOCSTI is deprecated. The update is to the console KL11 handler in pdp11_stddev such that
	a key event in the UI window is now injected into the KL11 (or remote) input stream. This is equivalent
	to having an LK40 keyboard linked to the 11/05 console KL11. Now, you can have the UI in focus and type
	into the screen. This still works in Windows. This update has been tested in Fedora 38 64 bit.
	Other new distros should be OK.
<br>
<br>
	Ian Schofield Oct 2023.
<br>
<br>
 	See above. This update to sim_video_gt40.c is significant. The display logic has been changed to take
	advantage of a number of useful features of SDL2. However, this also gets into game territory where
	anything less that 180 FPS means you have no chance! The display update is now frame synced and called
	from Mloop(). Now, Refresh() is slaved to Mloop() rather then the other way around. This considerably
	reduces display flicker and shading. This has been tested in Fedora 38 64bit and Windows. The app
	will also work on a Pi. I have tried Bookworm 64bit on a Pi4 to do this. But, the app really hammers the CPU
	so I cannot recommend this device.
	(For the Pi app to work, you need: export SDL_VIDEODRIVER=x11 and export SDL_VIDEO_X11_XRANDR=0).
	As this update may cause some problems, I have left a previous copy of sim_video_gt40.c in the repo
<br>
<br>
	Ian Schofield Oct 2023.
<br>
<br>
   	This build has some further updates to the display system. The Refresh() function is now called from Mloop()
  	which is synced to the monitor frame rate. Also, the original sync system in vt11.c has been disabled such
  	that the VT11 hardware always runs as fast as possible. The effect of this is patricularly noticeable
  	using the VT07 graphics app which uses a software character builder and is line synced. The line sync
  	logic as above was always only an approximation to the true line frequency. Now that this is disabled,
  	the VT07 display no longer flickers.
  	As above, the app performance is still borderline on a Pi4. It may be that it is useable on a Pi5.
<br>
<br>
	Ian Schofield Nov 2023.<br>
 <br>
 <br>
 	Yet another minor change to the display system so that the app works on a Mac. This was tested in a VM
	running Sonoma. This was rather hard work as SDL works slighly differently.
<br>
<br>
	Ian Schofield Nov 2023<br>
 <br>
 <br>
 	This update adds some more versions of spacewar and a way of presetting the SR from SIMH. To do this
	use 'DEP SR xxxxx' before the 'SET VT ENA' command.<br>
 	Please note that if you wish to try the GTLOAD option to connect to ITS, use the vt07 script
	and change lines 17 and 18 to 'boot rom0'. Using this option can cause a VT11 trap. If this
	occurs, change line 65 in PDP11/pdp_vt.c to '#define VT11_DELAY 20'. This trap  occurs because
	the bootvt programme cannot keep up with the VT11 instruction fetches.
	In addition, I have included a script to run the VT11 embedded Lunar Lander. 'do lunar'.
	I have added two versions of Spacewar.
	Run with 'do spacewar'. This is from Lars Brinkhoff and I am not sure how it works.
	Run with 'do spwar'. I do not know where this is from but it uses the SR to control the ships. (Really hard!).<br>
   	Finally as the amazing PiDP10 has arrived, I have added a specific ITS script. Use as 'do its (DNS name of your PiDP10)'.
  	This will attach the GT40 to port 10019 and allow it to be used as a LLOGO display slave.<br>
   	sim_txmr.c has been updated so that the telnet negociation now works.
<br>
<br>
	Ian Schofield Mar 2024<br>
 <br>
 	This patch is to synchronise the screen update from the SDL2 code in MLoop() (sim_video_gt40.c)<br>
	In addition, this corrects a crash that can occur using the bootvt.img rom bootstrap.<br>
 	To try/use this boostrap code see the its script. The boot image contains code that may
	be used to load binaries from ITS using the :GTLOAD command.<br>
 	However, I would note that the Lunar Lander ITS binary never gets past the start screen.<br>
  	To correct this, once the start screen is on the display, stope simh from the GT40 console (^e)<br>
   	then type the following into simh:  d psw 0 and then c. After a few seconds Lunar Lander will start.<br>
    <br>
    	Ian Schofield Jun 2024<br>
     
 
 


