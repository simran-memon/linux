CMPE283 – VIRTUALIZATION TECHNOLOGIES

ASSIGNMENT – 2 : Instrumentation via hypercall

Goal: To modify the CPUID emulation code in KVM to report back additional information 
when special CPUID leaf nodes are requested.

	•For CPUID leaf node %eax=0x4FFFFFFF:
		◦Return the total number of exits (all types) in %eax
	•For CPUID leaf node %eax=0x4FFFFFFE:
		◦Return the high 32 bits of the total time spent processing all exits in %ebx
		◦Return the low 32 bits of the total time spent processing all exits in %ecx
			▪%ebx and %ecx return values are measured in processor cycles, across all VCPUs
			
Question_1: For each member in your team, provide 1 paragraph detailing what parts of the lab that member 
implemented / researched.	

=> Simran - SJSU_ID: 015950610

           Researched about exits reasons NOT enabled/handled in SDM
	   
           Installing KVM QEMU and Ubuntu OS as a Nested VM 
	   
           Build the linux kernel
	   
           Modified code in cpuid.c
	   
           Testing the setup by a test program
	   
            
=> Limeka  - SJSU_ID: 015905812

           Researched about exits reasons NOT enabled/handled in KVM
	   
           Researched and implemented atomic variables
	   
           Build the linux kernel
	   
           Modified code in vmx.c
	   
           Worked on fixing runtime issues
	   
		
Question_2: Describe in detail the steps you used to complete the assignment. 
=>

Step_1: Modified the kernel code into two files over host(Ubuntu) machine at below location:

        ~linux/arch/x86/kvm/cupid.c
        ~linux/arch/x86/kvm/vmx/vmx.c

Step_2: Build the kernel with the following commands:

        -> cd linux
        -> make -j 16 modules ( // comment out the trusted key in .config file, since the make process will create a new key at every
           build)
        -> make -j 16
        -> sudo bash
        -> sudo make INSTALL_MOD_STRIP=1 modules_install && make install ( // this will create a new installer and install kernel version)
        -> uname -a ( // check updated kernel version on the host VM )
        -> sudo reboot ( // this reboots the VM for the changes to take place)

Step_3: Install KVM-QEMU (hypervisor) on the host VM with the help of following commands:

        -> sudo apt install virtinst
        -> sudo apt install libvirt-clients
        -> sudo apt install virt-top
        -> sudo apt install qemu-kvm
        -> sudo apt install libvirt-daemon
        -> sudo apt-get update -y
        -> sudo apt-get install -y libvirt-daemon-system
        -> sudo apt install bridge-utils
        -> sudo apt install virt-manager
        -> sudo virt-manager ( // this will launch the virt manager or can be opened from the application section)
        -> Now install the Ubuntu OS as a nested VM through the Virt Manager and perform a reboot.
        -> You will now be able to login into the inner VM.
        
	
![alt text](https://github.com/simran-memon/linux/blob/master/assignment_2_3_screenshots/VM-into-VM.png?raw=true)
        
        
Step_4: Testing the output for first two leaf nodes on the inner VM now

        -> Install cpuid using : sudo apt install cpuid
        -> After installing cpuid, to check for leaf node 0x4FFFFFFF execute in inner VM: cpuid -l 0x4FFFFFFF
           
           output:
 
        
![alt text](https://github.com/simran-memon/linux/blob/master/assignment_2_3_screenshots/LEAF_NODE_F.png?raw=true)
        
        -> To check for leaf node 0x4FFFFFFE, execute in inner VM: cpuid -l 0x4FFFFFFE
     
          output:
     
![alt text](https://github.com/simran-memon/linux/blob/master/assignment_2_3_screenshots/LEAF_NODE_E.png?raw=true)
        
        
        
==================================================================================================================================================================




ASSIGNMENT – 3: Instrumentation via hypercall (cont’d)

Goal: To modify the CPUID emulation code in KVM to report back additional information 
when special CPUID leaf nodes are requested.

	•For CPUID leaf node %eax=0x4FFFFFFD:
		◦Return the number of exits for the exit number provided (on input) in %ecx
		▪This value should be returned in %eax 
	•For CPUID leaf node %eax=0x4FFFFFFC:
		◦Return the time spent processing the exit number provided (on input) in %ecx
		▪Return the high 32 bits of the total time spent for that exit in %ebx
		▪Return the low 32 bits of the total time spent for that exit in %ecx
	•For leaf nodes 0x4FFFFFFD and 0x4FFFFFFC, if %ecx (on input) contains a value not defined by the 
	SDM, return 0 in all %eax, %ebx, %ecx registers and return 0xFFFFFFFF in %edx. For exit types not 
	enabled in KVM, return 0s in all four registers.

Question_1: For each member in your team, provide 1 paragraph detailing what parts of the lab that member 
implemented / researched.	

=> Simran  - SJSU_ID: 015950610

           Researched about exits reasons NOT enabled/handled in SDM
	   
           Checking growth rate of exit types
	   
           Build the linux kernel
	   
           Modified code in cpuid.c
	   
           Testing code changes
	   
            
=> Limeka  - SJSU_ID: 015905812

           Researched about exits reasons NOT enabled/handled in KVM
	   
           Checking total exits after nested VM reboot
	   
           Build the linux kernel
	   
           Modified code in vmx.c
	   
           Handled error conditions and debugging issues
	   
           
           
Question_2: Describe in detail the steps you used to complete the assignment. 

=>
Step_1: Modified the kernel code into two files over host(Ubuntu) machine at below location:

        ~linux/arch/x86/kvm/cupid.c
        ~linux/arch/x86/kvm/vmx/vmx.c

Step_2: Build the kernel with the following commands:

        -> cd linux
        -> make -j 16 modules ( // comment out the trusted key in .config file, since the make process will create a new key at every
           build)
        -> make -j 16
        -> sudo bash
        -> sudo make INSTALL_MOD_STRIP=1 modules_install && make install ( // this will create a new installer and install kernel version)
        -> uname -a ( // check updated kernel version on the host VM )
        -> sudo reboot ( // this reboots the VM for the changes to take place)
      
Step_3: Testing the output for last two leaf nodes on the inner VM now

        -> To check for leaf node 0x4FFFFFFD execute in inner VM: cpuid -l 0x4FFFFFFD -s 0
	   
	   output:
	   
![alt text](https://github.com/simran-memon/linux/blob/master/assignment_2_3_screenshots/LEAF_NODE_D.png?raw=true)
        
        -> To check for leaf node 0x4FFFFFFC, execute in inner VM: cpuid -l 0x4FFFFFFC -s 0
        
           output:           
                             
![alt text](https://github.com/simran-memon/linux/blob/master/assignment_2_3_screenshots/LEAF_NODE_C.png?raw=true)
        
Question_3: Comment on the frequency of exits – does the number of exits increase at a stable rate? Or are there 
more exits performed during certain VM operations? Approximately how many exits does a full VM 
boot entail?

=> The growth rate of the number of exits does not seem to be stable. For example, exit codes from 56 - 69, all exited 0 times. Whereas, with some of the VM instructions like wget, unauthorized file access or other VM operations being executed inside the guest VM more number of exits occur. This could be due to exceptions, page fault, etc. For exit reason 1 (external interrupt) the count increased rapidly from 3946878 to 4248001 when a "wget http://www.google.com/" command was executed on the nested VM.

=> For our current setup, before the reboot the count was: 0x0019e6d8   => 1697496 (Decimal) and after a reboot on nested VM the count increased to: 0x002bf82f    => 2881583 (Decimal), approximately 1184087 exits occur.


Question_4: Of the exit types defined in the SDM, which are the most frequent? Least?

=> The exit reason 48 (EPT Violation) seems to be the most frequent exit type. Also, exit reason 1 (External interrupt), exit reason 10 (CPUID), exit reason 30 (I/O Instructions), exit reason 32 (WMR) occur very frequently. On the other hand, exit reason 29 (MOV DR) occurred only once and exit reasons 54 and 55 twice.
        
        
        
        
        
       

        
        








