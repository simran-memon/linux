CMPE283 – VIRTUALIZATION TECHNOLOGIES

ASSIGNMENT – 1 : DISCOVERING VMX FEATURES

GITHUB: https://github.com/simran-memon/linux


Following are the steps involved in creating Linux kernel module which displays 5 mentioned MSRs to determine virtualization features available in my CPU:

PREREQUISITES:

1. HOST OPERATING SYSTEM: Windows11 x64 
2. RAM: 16 GB
3. STORAGE: 256 GB
4. PROCESSOR: Intel® Core(TM) i7 11th generation

INSTALLATION:
1. Install VMware Workstation 16 Player on Host OS
2. Install Ubuntu 64-bit 20.04.3 LTS as Guest OS
3. After the installation is completed, enable hardware assisted virtualization as follows:
	→ Go to VM Machine Settings
	→ Go to Hardware
	→ Select ‘Processors’ from the list
	→ Enable first check box under Virtualization engine: 'Virtualize Intel VT x/EPT or AMD-V/RVI'
4. Install git, gcc(build-essential), ncurses, pkg-config, flex, libssl-dev, bison
5. Now fork the torvalds linux repository to your account
6. Git clone your own repository: https://github.com/simran-memon/linux.git
7. Update the configuration file to get the latest cloned version of Linux
8. Install all the modules and the kernel code.
9. Now reboot the VM to check if the latest Linux version is updated.
10. Create a directory named ‘cmpe283’ under directory : ‘linux’.
11. Copy files: ‘Makefile’ , ‘cmpe283-1.c’ to directory: ‘cmpe283’.
12. Add the 5  MSRs to display their features, along with whether their value can be set or cleared.
13. Add:  " MODULE_LICENSE(“GPL v2”); "   at the very end of the cmpe283-1.c file.
14. Execute command: make , this will show that the build was successful and a kernel object: cmpe283-1.ko is created
15. After the successful build, load the file to the kernel using command: ‘sudo insmod cmpe283-1.ko’
16. After loading the file, execute command: ‘dmesg’ to receive the output. This will display all the mentioned 5 MSRs along with the value for their set and cleared.
17. Now execute command: ‘sudo rmmod cmpe283-1’ to load file out of the kernel.
18. Add the files to your git repository: ‘git add Makefile cmpe283-1.c’
19. Commit the files: ‘git commit Makefile cmpe283-1.c’
20. And finally push the files to git: ‘git push’

To install make: sudo apt install make

To install gcc: sudo apt update
                sudo apt install build-essential
                sudo apt-get install manpages-dev
                gcc –version

To install ncurses: sudo apt-get install libncurses5-dev libncursesw5-dev

Output:

![alt text](https://github.com/simran-memon/linux/blob/master/assignment%231_screenshots/first.png?raw=true)
 

Pinbased Controls MSR & Primary Proc-Based Controls MSR:

![alt text](https://github.com/simran-memon/linux/blob/master/assignment%231_screenshots/second.png?raw=true)


Secondary Proc-Based Controls MSR & Exit Controls MSR:

![alt text](https://github.com/simran-memon/linux/blob/master/assignment%231_screenshots/third.png?raw=true)

Entry Controls MSR:

![alt text](https://github.com/simran-memon/linux/blob/master/assignment%231_screenshots/fourth.png?raw=true)

Complete Output: https://github.com/simran-memon/linux/blob/master/assignment%231_screenshots/Output.txt
