# Definition
A **hypervisor**, also known as a **virtual machine monitor** (**VMM**) or **virtualizer**, is a type of computer software, firmware or hardware that creates and runs virtual machines. It separates VMs from each other logically, assigning each its own slice of the underlying computing power, memory and storage.

> Hypervisor and virtual machine (VM) are related but distinct concepts in virtualization technology.

A hypervisor allows one host computer to support multiple guest VMs by virtually sharing its resources, such as memory and processing.

[vmware官网关于hypervisor的介绍](https://www.vmware.com/topics/glossary/content/hypervisor.html?resource=cat-2143999149#cat-2143999149)
[ibm网站的介绍](https://www.ibm.com/topics/hypervisors)

# Why hypervisor?
Before hypervisors hit the mainstream, most physical computers could only run one operating system (OS) at a time. This made them stable because the computing hardware only had to handle requests from that one OS. The downside of this approach was that it wasted resources because the operating system couldn't always use all of the computer's power.

A hypervisor solves that problem. It is a small software layer that enables multiple instances of operating systems to run alongside each other, sharing the same physical computing resources. This prevents the VMs from interfering with each other; so if, for example, one OS suffers a crash or a security compromise, the others survive.

# Classification
There are two types of hypervisors
- Type-1, native or bare-metal hypervisors
- Type-2, hosted hypervisors

![[hypervisor_classification.png]]


