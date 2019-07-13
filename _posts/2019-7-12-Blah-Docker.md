---
layout: post
title:  "Docker"
categories: Blah
---
# What's a VM
VM is a generally vitual OS, so it simulates output and input of hardware in software level, so the operations of VM can be running in a hardwareless environment. Hypervisor manages how to create and manage VM, it is also called VMM(virtual machine manager).

## Type1 Hypervisor: Hardware --> Hypervisor --> OS
Type 1 hypervisors are regarded as the most efficient and best-performing hypervisors available for enterprise computing. Hypervisors such as VMware ESXi, Microsoft Hyper-V server and open source KVM are examples of Type 1 hypervisors. 

## Type2 Hypervisor: Hardware --> OS --> Hypervisor --> VM OS
Guest OS --> Application --> HyperVisior --> Hardware of a VM(CPU, memory, NIC(network interface controller), Disk). 


Example:Hyper-V, codenamed Viridian, formerly known as Windows Server Virtualization, is a native type2 hypervisor running on windows; This is how docker runs in Windows

It is low performance because of it has lots of overhead. But the new technology now supports hardware IO redirect. But they need to use same Instruction Set Architecture(ISA) because the requests for CPU is redirect directly.


# Why Docker
When a VM boots up, it needs to start its kernel and user processes, it takes a long time. But if the host and VM have same kernel, then we just need to redirect kernel IO, not hardware IO, it is called Operating-system-level virtualization, container.

让我们来回顾虚拟机的概念，在虚拟机的系统中，虚拟机认为自己有独立的文件系统，进程系统，内存系统，等等一系列，所以为了让容器接近虚拟机，也需要有独立的文件系统，进程系统，内存系统，等等一系列，为了达成这一目的，主机系统采用的办法是：只要隔离容器不让它看到主机的文件系统，进程系统，内存系统，等等一系列，那么容器系统就是一个接近虚拟机的玩意了

# What's kernel
A kernel is the central part of an operating system. It manages the operations of the computer and the hardware - most notably memory and CPU time. Its one layer above driver.

There are two types of kernels:

* A micro kernel, which only contains basic functionality;
* A monolithic kernel, which contains many device drivers.

A computer user never interacts directly with the kernel. It runs behind the scenes and cannot be seen, except for the text logs that it prints.

#### Operations of kernel

The kernel is the most fundamental part of an operating system. It can be thought of as the program which controls all other programs on the computer. When the computer starts, it goes through some initialization (booting) function, such as checking memory. It is responsible for assigning and unassigning memory space which allows software to run.

It provides services so programs can request the use of the network card, the disk or other piece of hardware (the kernel forwards the request to special programs called device drivers which control the hardware), manages the file system and sets interrupts for the CPU to enable multitasking. Many kernels are also responsible for ensuring that faulty programs do not interfere with the operation of others, by denying access to memory that has not been allocated to them and restricting the amount of CPU time they can consume. It is the heart of the operating system.

<hr>

# Docker containers have their own kernel or not?

Docker uses host OS kernel, there is no custom or additional kernel inside container. All containers which run on a machine are sharing this "host" kernel.

# Docker creation
(知乎-提高篇)[https://zhuanlan.zhihu.com/p/22403015]

