# CMPE283-assignment-1
## Question:
Describe in detail the steps you used to complete the assignment. Consider your reader to be someone skilled in software development but otherwise unfamiliar with the assignment. Good answers to this question will be recipes that someone can follow to reproduce your development steps.

## Answer:
Used Nested Virtualization in Google Cloud Platform (GCP), Nested virtualization lets us to run virtual machine (VM) instances inside of other VMs so that we can create our own virtualization environments.
Compute Engine VMs run on a physical host that has Google's security-hardened, KVM-based hypervisor. With nested virtualization, the physical host and its hypervisor are the level 0 (L0) environment. The L0 environment can host multiple level 1 (L1) VMs. On each L1 VM is another hypervisor, which is used to install the level 2 (L2) VMs

The following steps followed to develop and test the kernel module:
1.	Create a GCP Free Tier Account which provides $300 USD Free credit to try various GCP Services.
2.	You need a Credit/Debit card with international transactions enabled for verification purposes only. They will charge $1 USD while registration and then refund it.
3.	Create a boot disk from a public image or from a custom image.

 ![image](https://user-images.githubusercontent.com/40047632/198859737-a847b611-362f-4388-98f1-0a98d4257270.png)



4.	Open the Cloud Shell by clicking on the icon at the top right corner.


5.	Create a custom image with the special license key that is required for nested virtualization.\
gcloud compute images create IMAGE_NAME \
  --source-disk DISK_NAME \
  --source-disk-zone ZONE \
  --licenses https://www.googleapis.com/compute/v1/projects/vm-options/global/licenses/enable-vmx
6.	Optional, delete the source disk after creating the image with the special license.
7.	Create a VM that uses the new image with the special license.
i.	Name: For example “instance-1”.
ii.	Region: Select the Region where the custom image was created. example "us-west1-b"
iii.	Machine Family: General Purpose
iv.	Series: Select "N2" for lab
v.	Machine Type: Select “N2-standard-8 (8 vCPU, 32 GB Memory)” → This is recommended for lab setup; however you can select any other combination as per your requirement.
vi.	Then click the 'Change' button for the Boot Disk selection. You need to select your custom image "cmpe-283-image".
vii.	Then select the Firewall settings from the main screen and Select both HTTP and HTTPS traffic.
viii.	Click on Create button to create the Linux instance.

![image](https://user-images.githubusercontent.com/40047632/198859746-3bc3ac78-d2e3-445f-8d24-eea09dfa2cf0.png)

 

![image](https://user-images.githubusercontent.com/40047632/198859752-872d8365-0a1a-4758-9f9a-850e8628f4b4.png)
 

 

8.	Now you can access the VM directly from the browser through SSH.
9.	Create a new directory named "cmpe283-assing".
>> mkdir cmpe283-assing
10.	Copy the template "cmpe283-1.c" file and the template "Makefile" provided by the professor to the cmpe283-assing directory.
11.	Modify “cmpe283-1.c" file, add all others 5 MSRs as explained in the assignment description:
i.	By referring SDM, created structures with name (description) and bit positions for Primary Processor based, Secondary Processor based, Tertiary Processor, Entry and Exit controls.
ii.	In order to detect and print VMX capabilities of CPU, the function report_capability ( ) is called with appropriate parameters passed in order to print Primary Processor based, Secondary Processor based, Tertiary Processor, Entry and Exit controls.
12.	Run below command:
>> apt install gcc make
>> sudo apt-get linux-headers-$(uname -r)
13.	Build the module using the following command inside the cmpe283-assing directory.
>> make
14.	Module is inserted into the kernel:
>> sudo insmod ./cmpe283-1.ko
15.	Verify that the VMX capabilities for all the MSRs are displayed in the logs command:
>> sudo dmesg
16.	when the module is removed with command:
>> sudo rmmod cmpe283-1

## Output of dmesg Logs:

![image](https://user-images.githubusercontent.com/40047632/198859766-784f45e2-7fc1-4b65-acf6-40831041cad8.png)

![image](https://user-images.githubusercontent.com/40047632/198859773-7639417f-3ed0-440c-85d3-fe417f1184dc.png)

![image](https://user-images.githubusercontent.com/40047632/198859775-b2990e98-f354-4a13-b1c1-2987faff5d76.png)

