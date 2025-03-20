### README

Topics:
- Using WSL (Windows Subsystem Linux) to build software for an embedded linux platform.
- Specific embedded platforms/targets, like Zynq 7000 and UltraScale+
- Specific tools, like AMD (formally Xilinx) Vitis
- Linux virtualization using WSL and Dockers for cross-compiling, etc

#### Reference Links:

##### WSL:

- https://www.omgubuntu.co.uk/how-to-install-wsl2-on-windows-10 (How to Install WSL 2 on Windows 10 (Updated))

- https://superuser.com/questions/1746633/update-the-windows-subsystem-for-linux-wsl-to-wsl-2 (Update the Windows Subsystem for Linux (WSL) to WSL 2)

- https://youtu.be/qPMsV1DSGJY (Dr. Hayes - Running linux within windows with WSL2 course intro)

- https://youtu.be/vxTW22y8zV8 (WSL Chuck Network)

##### Embedded Linux on Zynq processor boards:

- https://www.youtube.com/watch?v=Xn5cs9uh8gc (Embedded Linux for Zynq 7000 / ZU+. Boot image *part 1*)
	- See 'Detail A' for details.
  
- https://youtu.be/PxGK7h_DYfE?si=ndtnCd4OWfOxJSO- (Embedded Linux for Zynq 7000 / ZU+. Boot image *part 2*. Buildroot for Zynq 7000 / Zynq Ultrascale+)
	- See 'Detail B' for details.
	
- https://youtu.be/V0nPAPBgUvU (Vivado HLS: an example of h/w acceleration for Zynq 7000/Utrascale+ *part 3* - s/w vs h/w)

- https://www.youtube.com/watch?v=Si1VfsejzdU  (How to build Embedded Linux for Zynq 7000, Zynq Ultrascale+ with Vitis 2022.1 and Buildroot)

- https://youtu.be/ZyBBv1JmnWQ (How to install Docker on Windows - 2025 [ step by step guide ])

- https://stackoverflow.com/questions/33848947/accessing-docker-container-files-from-windows (Accessing Docker container files from Windows)

- https://developer.arm.com/downloads/-/arm-gnu-toolchain-downloads (?)

- https://www.amd.com/en/developer/resources/vitis.html (Xilinx is now AMD? AMD Vitis Software)
	- https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/vitis.html (Vitis Embedded Installer - supports Zynq™ MPSoC, Zynq 7000, ...)

- TFTP servers (so target can used 'tftpb' to load images):
	- https://www.solarwinds.com/free-tools/free-tftp-server
	- https://sourceforge.net/projects/tftputil/ (very old)
	
Command summary:

	cmd (run as administator)
		wsl --list --online
		wsl --install  or  wsl --install -d <distro>
		wsl --install -d ubuntu
    		(let install and then you have to reboot your PC
     			after reboot, will be in command shell of Ubuntu (I didn't have to?!)
     			will choose new username and password...
     			should end up logged in to you linux account! )

Install session:

	  C:\Windows\System32>wsl --install -d ubuntu
		Downloading: Ubuntu
		Installing: Ubuntu
		Distribution successfully installed. It can be launched via 'wsl.exe -d Ubuntu'

Launch session from terminal/cmd window with admin:

	C:\Windows\System32>wsl.exe -d Ubuntu

	Provisioning the new WSL instance Ubuntu
  	This might take a while...
  	Create a default Unix user account: wlaba
  	New password: ****
  	Retype new password:
  	passwd: password updated successfully
  	To run a command as administrator (user "root"), use "sudo <command>".
  	See "man sudo_root" for details.

  Welcome to Ubuntu 24.04.1 LTS (GNU/Linux 5.15.167.4-microsoft-standard-WSL2 x86_64)

   * Documentation:  https://help.ubuntu.com
   * Management:     https://landscape.canonical.com
   * Support:        https://ubuntu.com/pro

   System information as of Sun Feb 16 20:04:40 EST 2025

    System load:  0.0                 Processes:             31
    Usage of /:   0.1% of 1006.85GB   Users logged in:       0
    Memory usage: 5%                  IPv4 address for eth0: 172.23.133.253
    Swap usage:   0%

	This message is shown once a day. To disable it please create the /home/wlaba/.hushlogin file.
	wlaba@DESKTOP-ORAV2LD:/mnt/c/Windows/System32$
  
	ubuntu> sudo apt update (will need to enter your account password whenever 'sudo')
		sudo apt upgrade
		(reboot your virtual machine)
		wsl --list -v (to see state as 'running' or 'stopped')
		wsl --shutdown (shutdown ALL your VMs)
		
#### Get lastest WSL updates:

	windows cmd with admin access
	C:\Windows\System32>wsl --update
		Checking for updates.
		The most recent version of Windows Subsystem for Linux is already installed.	

#### Access windows files from within linux:

	VSCODE installed in windows (but not in Ubuntu!) yet still can launch!
  	- 'code testVsCode.txt' (needed to allow host 'wsl.wlaba~' for vscode app!)
	- file was saved in Ubuntu>home>wlaba (this is in our linux FS, not windows FS)

	wlaba@DESKTOP-ORAV2LD:~$ pwd
		/home/wlaba
	wlaba@DESKTOP-ORAV2LD:~$ ls
		testVsCode.txt
	If you ever need to get to your linux 'home' then, 'cd ~'
	  
	what about getting to an existing windows file?...
		wlaba@DESKTOP-ORAV2LD:~$cd /mnt
		wlaba@DESKTOP-ORAV2LD:~$ ls /mnt
		c  wsl  wslg  (the 'c' is your windows c: drive!)

	wlaba@DESKTOP-ORAV2LD:~$ cd /mnt/c/ (and tab key.. shows all folders in my windows OS!)
		$Recycle.Bin/              OneDriveTemp/              System Volume Information/
		$Windows.~WS/              PerfLogs/                  Users/
		Config.Msi/                Program Files/             Windows/
		Documents and Settings/    Program Files (x86)/       msys64/
		Drivers/                   ProgramData/               ti/
		ESD/                       Projects/                  ti_newStuff_JacintoEVM/
		Intel/                     Recovery/                  tirtos_workshop/

	wlaba@DESKTOP-ORAV2LD:~$ cd /mnt/c/Users/wlaba/ (tab shows my windows user account folders!)

	If space in path, need slash or single quotes:
		example: cd Source\ Code   or   cd 'Source Code'
	
	Set a link (shortcut) to a windows FS folder:
		ln -s '/mnt/c/Users/wlaba/Documents/Source Code' codeInWindows 
			
#### Access linux files from within windows:

	Open up a windows file explorer... should now see 'Linux' icon!
	Click on it and browse to your linux home directory, 'Linux>Ubuntu>home>wlaba>'
		also known as '\\wsl.localhost\Ubuntu\home\wlaba'
		or if Linux does not show in explorer, try ' \\WSL$ ' in explorer url
		should now see your linux folders/files!
	
	Set a link (shortcut) to a linux FS folder (ex. 'SourceCode' folder under my linux home dir):
		In a cmd shell with admin
		mklink /d sourceInLinux \\wsl.localhost\Ubuntu\home\wlaba\SourceCode
		
#### Running graphical commands:

	Launch your ubuntu 
		wlaba@DESKTOP-ORAV2LD:~$ sudo apt install x11-apps
		(should proceed to install...)
		wlaba@DESKTOP-ORAV2LD:~$ xclock  (should see clock gui window...)
		wlaba@DESKTOP-ORAV2LD:~$ xcalc   (should see calculator gui window...)
		
		wlaba@DESKTOP-ORAV2LD:~$ sudo apt install gnuplot -y (useful code performance tool)
		(should proceed to install...)
		wlaba@DESKTOP-ORAV2LD:~$ gnuplot
			   :
			gnuplot> plot sin(x) with line (should see a sine wave plot in a gui window...)
			gnuplot> set title "Sine Wave"
			gnuplot> replot   (to see title added to plot)
			gnuplot> exit
			
#### Terminal in Windows 11:

	In windows start or search, type 'terminal'
	By default will run PowerShell, but see drop-down to select other tab/options, like command prompt and Ubuntu!
	Can use drop-down to select multiple instances of selected tabs!
	
#### Enable Virtualization:

	Via your BIOS on bootup?
    	- For Intel machines... Intel VT-X
    	- For AMD machines....	AMD-V
	
#### Windows Version: 

	Type 'winver' in windows search bar 
    - launches 'about windows' pop-up
	- I see Windows 11 version 24H2 (OS Build 26100.3194)

#### Launch linux distro from terminal/cmd window:

	wsl -d <distro>, ex. - wsl -d ubuntu (or look for 'ubuntu' under terminal dropdown tab and select)
	type 'exit' to exit linux
	type 'exit' to exit terminal/cmd window

	Can run 'windows command' in linux 
		try 'ipconfig.exe' (note .exe is needed)... works!
		try 'notepad.exe test.txt' ... works!
		try 'explorer.exe .' ... works!
									  

	Can run 'linux commands' in windows shell... try ' ipconfig | wsl grep 10. ' ... eh?! errors?! ... FIXED as follows...
    	wsl --list -v   showed 'docker-desktop-data' as default distro (note the '*' to the left)
		wsl -s Ubuntu	changed ubuntu to the default distro (now it has '*' to it's left) ... works!...
			PS C:\Users\wlaba> ipconfig | wsl grep 172
				IPv4 Address. . . . . . . . . . . : 172.23.128.1

#### Kali Linux (the hacking linux distro):

	wsl --list --online (terminal/cmd shell as admin)
		should see 'kali-linux' listed
		could install with 'wsl.exe --install kali-linux', BUT if this way has problems... use the Microsoft store!
		From search bar, type 'store' ... select 'Microsoft Store'... in store search, type 'kali linux'... should see Kali Linux selection to 'Get'
		Should install and see command window to set username/password... now you should be in your 'kali linux' distro session!
		Your kali linux distro starts out as a 'minimal install' with no tools... 
		Run the command 'kali-tweaks' from your kali linux shell... select 'Metapackages', enter your (sudo) password...
		Select 'top10' for 'Kali's top 10 tools' ... use down arrow/tabs/enter to apply, enter for OK... let install finish...

	sudo apt install spiderfoot (OSINT tool - Open Source Intelligence Tool for Hackers)

	spiderfoot -l 127.0.0.1:5001 (starts a webserver running on your wsl)
	launch webrowser and url to 'localhost:5001'
	!!! informercial... 'Truthfinder' website (similarly 'Peoplefinder') to see how your privacy is violated online... and 'Incogni' website to protect your privacy. 
	
	sudo apt install kali-win-kex -y
		after install...
		type 'kex'  (full blown kali linux desktop!)
		F8 key to go in/out full screen
		kex --win --stop

#### wsl command summary:
	
	wsl --list --online	
	wsl --install <distro>  (distro's listed in previous command)
	wsl --list -v (or --verbose)
	wsl --shutdown (to shutdown/stop all your distros)
	wsl --terminate <distro>  (shutdown/stop just this distro)
	wsl --set-default <distro> (sets this distro as your default - will see asterisk when verbose listed)
	wsl --status
	wsl --unregister <distro>  (to remove/uninstall this distro)

#### wsl configuration files:

	Local configuration:
		/etc/wsl.conf file in each of your distros
		refer to microsoft WSL Documentation pages for wsl.conf settings and wsl.conf example file
	
	Global configuration:
		.wslconfig file 
		settings will be used for every WSL instance you are running
		stored in your windows operating system inside your user directory
		not created by default
		To create in powershell/cmd shell: 'notepad.exe $env:USERPROFILE\.wslconfig' (make sure file doesn't have .txt extension!)
			Sample .wslconfig file contents:
			
				[wsl2]
				networkingMode=mirrored  (default networkingMode for WSL is NAT)
				
		wsl --shutdown    (8 second rule for changes to take affect!)
		wsl -d  <distro>  (to startup your distro again)
		
		type 'ifconfig' and you should now see your ip address match your windows ip address (no NAT)
		
	
#### When in your linux distro:

	cd /mnt/c  					(navigate to your windows c: drive)
	cd Users/wlaba/Downloads/	(navigate to your windos account download folder)
	ls -alt | grep .txt			(grep this folder for any txt files)
	cd ~ 						(navigate to your linux home)
	explorer.exe .				(open up your current folder in windows explorer)
		note location in explorer url is '\\wsl.localhost\Ubuntu\home\wlaba'
		note linux penguin icon in left pane of windows explorer!... quick access to all your distro filesystems!
		can copy/paste between your windows and linux distros filesystems EZPZ!
	nvidia-smi (can view your graphics card...)
	
	'code .' or 'code <filename>' to launch VSCODE!
	VSCODE has extension for WSL (can run linux distro terminals inside VSCODE!)

#### Backup your WSL instances:
	
	In PS/Cmd shell
	wsl --list --verbose  (to view and pick the distro you wish to backup)
	wsl --export <distro> <distro>.tar
	ls | wsl grep <distro> (should see the backup file)

	move to where you want it... say your D: drive?...
	wsl --import <name> D:\wsl .\<distro>.tar
	wsl --list  (should see <name> you gave for imported distro)
	wsl -d <name> -u wlaba (to launch/connect to your imported distro with specified user 'wlaba')
	
#### Can run DOCKER containers inside WSL:

	sudo apt install docker.io
	or can install 'docker desktop' from docker.com website

	(installed docker desktop via windows download/install... then back to distro shell...)

	sudo docker run -itd --name heybuddy busybox
	
	(go back to docker desktop and can now see your 'heybuddy' 'busybox' docker image!)
	
	Can also look at your docker containers in VSCODE... (install docker extension in VSCODE)
	Will see docker icon in VSCODE, click on it and will see your containers here as well!
	Select Terminal tab in VSCODE to attach to your container...
			
#### Embedded Linux on Zynq processor boards: 'Detail A'

    - https://xilinx-wiki.atlassian.net/wiki/spaces/A/pages/460653138/Xilinx+Open+Source+Linux (Xilinx Open Source Linux)
    - Vivado 2019.1 and launch of Xilinx SDK
    - Cross compilers (linux host): https://developer.arm.com/downloads/-/gnu-a 
	- Zynq 7000: 		AArch32 target with hard float (arm-none-linux-gnueabihf)(AArch32 target with hard float (arm-none-linux-gnueabihf))
	- Zynq UltraScale+: AArch64 GNU/Linux target (aarch64-none-linux-gnu)
    - Fetch sources: https://xilinx-wiki.atlassian.net/wiki/spaces/A/pages/18842156/Fetch+Sources\
	- High level folders on host (home/tools/):
		- arm-trusted-firmware (ATF)
		- devicetree
		- dtc (device tree compiler)
		- gcc-aarch64 (cross compilers)
		- u-boot-xlnx (u-boot and xilinx github)
	- Device Tree generator plugin for xsdk (must be for your xilinx SDK version)
		- https://github.com/Xilinx/device-tree-xlnx and drop-down from 'master' branch to your version.
		- Download to your Xilinx SDK
		- SDK Window-> preferences-> Xilinx SDK-> Repositories-> Local Repositories -> 'C:\Xilinx\device-tree-xlnx-xilinx-vxxxx.x'
	- Many build steps to get to boot image partitions:
		- bl31.elf (trusted firmware)
		- pmufw.elf
		- fsbl.elf (first stage boot)
		- u-boot.elf (second stage boot)
		- system.bit (bitstream)		

#### Embedded Linux on Zynq processor boards: 'Detail B'
	- Buildroot with out-of-tree (set of the Li
	nux image files from Buildroot): https://buildroot.org/downloads/manual/manual.html
		- Linux Image (Image.itb): Kernel (Image.gz), Device tree (devicetree.dtb), rootfs (rootfs.cpio.Izma)
		- https://gitlab.com/buildroot.org/buildroot/  (git clone https://gitlab.com/buildroot.org/buildroot.git)
			- buildroot (to repo root workspace folder)
			- adjacent to buildroot folder, create 'tmp' folder for our outputs
			- adjacent to buildroot folder, create 'zynqus_extern' folder for our customizations
			- https://buildroot.org/downloads/manual/manual.html#customize (recommended directory structure: board, configs, package)
				- and 'Config.in', 'exeternal.mk' and 'external.desc' files
			- note 'mandatory packages' must be installed 
			- note 'sudo apt install u-boot-tools' must be installed as well
#### AMD (formerlly Xilinx) Vitis Tutorials:
- https://github.com/Xilinx/Vitis-Tutorials/blob/2024.2/README.md
- https://github.com/Xilinx/Vitis-Tutorials/blob/2024.2/Getting_Started/README.md
- Download: https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/vivado-design-tools/2024-2.html


				
		
	
	




	








	
			
			
			
			
		
		
		
		
		





