# CMPE283-Assignment2
## Assignment 2: Instrumentation via hypercall

### Team Members
	•	Madhurima Subodh Dani (015261974)
	•	Shreemathi Duvvuri (015273102)

### [1] For each member in your team, provide 1 paragraph detailing what parts of the lab that member implemented / researched.
- Madhurima
  - Built the kernel
  - Installed required kernel modules in the VM
  - Understood and researched about the code of the assignment
  - Included the discussed changes in the code  
  - Created and compiled test files
- Shreemathi
  - Built the kernel
  - Rewatched lecture on canvas and understood steps to be performed
  - Researched about cpuid instruction
  - Understood where to make changes to implement the assignment
  - Created documentation and updated README.md

### [2] Steps Followed

- Base Setup
  - In Google Cloud Platform, spin up a VM as done in assignment 1
  <img width="1436" alt="image" src="https://user-images.githubusercontent.com/51197183/164878010-17bca73f-541b-4591-920e-ca6963ae5965.png">

  - Fork the Kernel code from GitHub: git clone https://github.com/torvalds/linux.git into ersonal repository
  - Clone the Kernel code from personal repository into GCP VM
  - Kernel Code Compilation
    - Enter into root mode by executing: sudo bash
    - Install required pacakages: apt-get install build-essential kernel-package fakeroot libncurses5-dev libssl-dev ccache bison flex libelf-dev
    - Check current os info: uname -a
    <img width="1341" alt="1" src="https://user-images.githubusercontent.com/51197183/164872937-3bc653bc-f6be-490a-b4e5-44cac71dac3d.png">

    - Change .config file: cp -v /boot/config-5.4.0-52-generic ./.config
    <img width="1156" alt="2" src="https://user-images.githubusercontent.com/51197183/164872971-3bcd4648-e62d-4aac-9d6a-d462a68ac8e4.png">

    - Make oldconfig file: make oldconfig
      - We need to enter yes/no for a long list of config questions. Went ahead with default options highlighted for each such question. Takes ~15-20 mins to feed answers.
     <img width="1344" alt="Screen Shot 2022-04-21 at 10 51 58 PM" src="https://user-images.githubusercontent.com/51197183/164873094-72f77558-96b9-4397-9c98-3c224c50c6bb.png">

    - make -j
    - sudo make modules
    - sudo make install
    	- It failed with error: Failed to generate BTF for vmlinux
        <img width="856" alt="sudo make install fails" src="https://user-images.githubusercontent.com/51197183/164873506-4ebedcb7-bef1-44b3-92f2-9e0ea084f07d.png">
	
	 - Executed below command as a fix to above error
	 <img width="1428" alt="sudo apt install dwarves" src="https://user-images.githubusercontent.com/51197183/164873522-16d0e9a5-c648-4132-a9ce-efba9398baf3.png">
	 
	 - Executed again: sudo make install
	 <img width="1016" alt="sudo make install" src="https://user-images.githubusercontent.com/51197183/164873547-e74e14d1-96a5-4655-bfe7-3d17504a09b3.png">

 
    - sudo make modules_install
    - reboot
    <img width="917" alt="reboot" src="https://user-images.githubusercontent.com/51197183/164873584-7710e632-824a-4c2a-a717-c1b57aae7b6c.png">

    - Verify updated Linux version: uname -a
    <img width="962" alt="uname -a" src="https://user-images.githubusercontent.com/51197183/164873612-63ff585d-1497-450d-ab9b-c8351ef249f3.png">


 - Update and Build cpuid.c and vmx.c files
   - cpuid.c file is loacted at linux/arch/x86/kvm/cpuid.c and vmx.c is located at linux/arch/x86/kvm/vmx/vmx.c
   - Modified cpuid.c and vmx.c files (refer updated files) in local and pushed to my git account
   - Cloned updated files into GCP VM via git
   <img width="729" alt="git clone assignment2" src="https://user-images.githubusercontent.com/51197183/164873853-6d24671b-1f9a-4f10-8371-e2ec87a417b0.png">

   - Compile code: sudo make -j && make modules && make install && make modules_install
   <img width="726" alt="image" src="https://user-images.githubusercontent.com/51197183/164873873-4b50fba6-b8de-487e-aab1-efe30b970d3f.png">

 - KVM Setup in GCP VM and spinning up Nested VM
   - Installed files needed to install virtual manager: sudo apt-get install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils
   - Installed virtual manager: sudo apt-get install virt-manager
   - Installed few more required modules: 
     - sudo apt-get install libvirt-bin libvirt-doc
     - sudo apt-get install qemu-system
   - Launching virtaul manager: sudo virt-manager
     - This step failed. Got connection refused message

   <img width="1304" alt="Screen Shot 2022-04-22 at 6 11 18 PM" src="https://user-images.githubusercontent.com/51197183/164874521-c73fe3dc-d01c-4fd0-baf2-108b44746361.png">

   - Downloded Ubuntu iso using wget 

   - Tried below virt-install command: It creates 
     <img width="1428" alt="image" src="https://user-images.githubusercontent.com/51197183/164874872-67349d6a-2221-4fd9-a538-6baa8cc834fe.png">
     
   - Listed out running domains: virsh list
    <img width="405" alt="image" src="https://user-images.githubusercontent.com/51197183/164874978-2fcd9074-7464-42e9-ae86-f9f3d7b78be3.png">

   - Connect with domain: virsh console domain-name
     - Didn't understand what can be done next here and how to execute commands in this domain.
     <img width="423" alt="image" src="https://user-images.githubusercontent.com/51197183/164875243-b77902de-301e-4ccf-90f1-e5598d135d36.png">

   - Left above approach. Understood that we can enable VNC to make the setup easier.
   
   - VNC Setup on GCP VM
     - Followed steps from:  https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-vnc-on-ubuntu-20-04
     - Downloaded VNC client on local desktop
     - Opened port 5901 in GCP Networks tab
     - Establised connection between VNC client and GCP VM
     - Executed sudo virt-manager from VNC client and it opened the uitility

   - Started Nested VM using the Ubuntu iso file and virtaul manager utility, 

     <img width="1393" alt="Screen Shot 2022-04-22 at 6 14 14 PM" src="https://user-images.githubusercontent.com/51197183/164874719-bea30e52-52be-4f33-9e36-61e5d6e5fc4a.png">
     
   - Compiled and executed test program

 
 ### [3] Comment on Exit Frequency
- Does the number of exits increase at a stable rate? Or are there more exits performed during certain VM operations?
  - No, the number of exits is not increasing at a steady rate. There are various VM instructions/operations that induce exits, such as EPT violation, RDRAND, I/O instruction, RDTSCP, and so on.

- Approximately how many exits does a full VM boot entail?
  - The number of exits after the first build, reboot and enter nested VM using the KVM is 187,420. This is not very accurate as there might have been a shutdown period and hardware interrupts in-between.

