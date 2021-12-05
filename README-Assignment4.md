CMPE283 – VIRTUALIZATION TECHNOLOGIES
ASSIGNMENT – 4 : Nested Paging vs. Shadow Paging
Goal: This assignment builds upon assignment 3. The purpose of this assignment is to illustrate the difference in performance when using nested paging versus shadow paging, and to illustrate the different exit frequencies and types.

Question_1: For each member in your team, provide 1 paragraph detailing what parts of the lab that member implemented / researched.
=> Since the setup was already done in assignment 2 and 3, we completed this assignment together in a zoom meeting. We performed a rerun of assignment 3 and booted a test VM to check the results(default nested paging, ept=1). Later, we removed the kvm-intel module and reloaded it with ept =0 (shadow paging) to check for the new results.
=> Simran - SJSU_ID: 015950610
Checked the output for nested paging with default ept=1
=> Limeka - SJSU_ID: 015905812
Checked the output for shadow paging with ept=0

Question_2:  Include a sample of your print of exit count output from dmesg from “with ept” and “without ept”.
=> Steps to be executed:
Run the assignment 3 setup, boot the test VM and note down the total exit count for CPUID leaf function CPUID -l 0x4FFFFFFD -s 0
After completion of above step, shutdown the test VM and then remove the kvm-intel module from running kernel using command : 
rmmod kvm-intel
Now reload the kvm-intel module again but this time pass the parameter ept=0 (This disables the nested paging and will force KVM to use shadow paging instead). Execute the following command:
insmod /lib/modules/5.15.0-rc7+/kernel/arch/x86/kvm/kvm-intel.ko ept=0
Boot test VM and check for the results by running ‘dmesg’ in the host VM.
=> With EPT (Nested Paging):

![alt text](https://github.com/simran-memon/linux/blob/master/assignment%234_screenshots/ept%3D1_nested_output.png?raw=true)

=> Without EPT (Shadow Paging, EPT=0):

![alt text](https://github.com/simran-memon/linux/blob/master/assignment%234_screenshots/ept%3D0_shadow_output.png?raw=true)

Question_3: What did you learn from the count of exits? Was the count what you expected? If not, why not?
=> The count of exits increased when nested paging was disabled(i.e. ept=0). So when KVM uses shadow paging the number of exits increases because exits could occur if VM attempts to execute CR0, CR3, CR4 or any exits which are related to paging such as a page fault.
Q4) What changed between the two runs (ept vs no-ept)?
=> After disabling the nested paging(i.e. ept=0), the inner VM was slow in the booting process. This is because nested paging directly performs a two-level page walk that makes TLB misses slower than non virtualized native, but enables fast page table changes. Alternatively, shadow paging restores native TLB miss speeds, but requires costly VMM intervention on page table updates. Also we observed that the exit counts for exit reasons 14 , 33 and 58 only appeared in shadow paging. The counts for exit reason 14 changed from 0 to 209650, exit reason 33 from 0 to 22690 and exit reason 58 from 0 to 1135.

