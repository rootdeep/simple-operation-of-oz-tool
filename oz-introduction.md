# Oz 工具简介

镜像制作一般分为手动制作和自动制作。

手动制作主要流程：

1. 利用OS镜像启动一个虚拟机实例，启动镜像的工具包括virt-install、virt-manager、qemu等；

1. 进入虚拟机实例，安装定制化软件包；

1. 压缩磁盘镜像并保存。

利用自动化工具制作：

自动化工具主要有： diskimage-builder, oz, image-bootstrap 等，它们主要通过配置文件完成虚拟机镜像制作。利用配置文件的好处主要有：

1. 一次手动配置保存为配置文件，以后反复使用，避免机械式的重复工作;
2. 避免手动步骤的遗忘。

在此介绍利用oz 工具进行自动化制作的方法。

Oz是一款基于Python的镜像制作工具，通过libvirt 和KVM完成操作系统，及定制化软件包的安装，支持多个操组作系统的安装。基于每一种系统，Oz都分三个阶段以完成定制化操作系统，这三个阶段是：操作系统安装、定制化软件安装、生成操作系统的软件包清单。

利用oz自动创建镜像需要两个配置文件：模板文件 *.tdl  和 kickstart (Red Hat-based systems) 或者preseed((Debian-based systems)。tdl文件是一个xml格式的文件，它确定了OS镜像的来源。 通常，我们在安装操作系统的过程中，需要大量的和服务器交互操作，为了减少这个交互过程，kickstart(简称ks)就诞生了。在ks 文件中记录了人工干预时填写的各种参数，如果在自动安装过程中出现要填写参数的情况，安装程序首先会去查找ks文件，如果找到合适的参数，就采用所找到的参数，找不到就合适的参数，使用默认值。其次，在ks 文件中，我们还可以添加需要安装的定制化软件包及其他脚本文件，这些定制化的步骤将在安装的第二个阶段完成。我们要做的就是告诉oz 从何处拿到tdl 文件和ks 文件，然后就去忙自己的事，等安装完毕。

更多Oz 介绍请参考：

Oz 介绍：https://github.com/clalancette/oz/wiki/Oz-architecture

Oz 支持的操作系统：https://github.com/clalancette/oz/wiki

下面以制作一个简单的虚拟机镜像为例，说明定制化虚拟机镜像的制作流程。