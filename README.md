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

- Firstly, the configuration set up for VM in assignment 1 has to be present.

 more steps
 
 
 ### [3] Questions
 -For whichever assignment you chose to complete CPUID 0x4FFFFFFD and 0x4FFFFFFC:
  - Comment on the frequency of exits – does the number of exits increase at a stable rate? Or are there  more exits performed during certain VM operations Approximately how many exits does a full VM boot entail?
  - Ans. The number of exits do not increase at stable rate. 
  - Of the exit types defined in the SDM, which are the most frequent? Least?
