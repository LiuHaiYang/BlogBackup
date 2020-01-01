---
title: Kubernetes
date: 2018-10-21 22:49:03
tags:
---
#### kubernetes
 - kubernetes，简称K8s，是用8代替8个字符“ubernete”而成的缩写。
 - 是一个开源的，用于管理云平台中多个主机上的容器化的应用，Kubernetes的目标是让部署容器化的应用简单并且高效（powerful）
 - Kubernetes提供了应用部署，规划，更新，维护的一种机制
 - 传统的应用部署方式是通过插件或脚本来安装应用,这样做的缺点是应用的运行、配置、管理、所有生存周期将与当前操作系统绑定，这样做并不利于应用的升级更新/回滚等操作，当然也可以通过创建虚拟机的方式来实现某些功能，但是虚拟机非常重，并不利于可移植性。
 - 新的方式是通过部署容器方式实现，每个容器之间互相隔离，每个容器有自己的文件系统 ，容器之间进程不会相互影响，能区分计算资源。相对于虚拟机，容器能快速部署，由于容器与底层设施、机器文件系统解耦的，所以它能在不同云、不同版本操作系统间进行迁移。
 - 容器占用资源少、部署快，每个应用可以被打包成一个容器镜像，每个应用与容器间成一对一关系也使容器有更大优势，使用容器可以在build或release 的阶段，为应用创建容器镜像，因为每个应用不需要与其余的应用堆栈组合，也不依赖于生产环境基础结构，这使得从研发到测试、生产能提供一致环境。类似地，容器比虚机轻量、更“透明”，这更便于监控和管理。

#### Kubernetes概述
 - Kubernetes是Google开源的一个容器编排引擎，它支持自动化部署、大规模可伸缩、应用容器化管理。
 - 在生产环境中部署一个应用程序时，通常要部署该应用的多个实例以便对应用请求进行负载均衡。
 - 在Kubernetes中，我们可以创建多个容器，每个容器里面运行一个应用实例，然后通过内置的负载均衡策略，实现对这一组应用实例的管理、发现、访问，而这些细节都不需要运维人员去进行复杂的手工配置和处理。

#### Kubernetes 特点编辑
 - 可移植: 支持公有云，私有云，混合云，多重云（multi-cloud）
 - 可扩展: 模块化, 插件化, 可挂载, 可组合
 - 自动化: 自动部署，自动重启，自动复制，自动伸缩/扩展

#### Kubernetes 组件编辑
 - Master 组件
   - kube-apiserver
   - ETCD
   - kube-controller-manager
   - cloud-controller-manager
   - kube-scheduler
   - 插件 addons
      - DNS
      - 用户界面
      - 容器资源监测
      - Cluster-level Logging
 - 节点（Node）组件
   - kubelet
   - kube-proxy
   - docker
   - RKT
   - supervisord
   - fluentd


#### Master 组件
- Master组件提供集群的管理控制中心。
	- Master组件可以在集群中任何节点上运行。但是为了简单起见，通常在一台VM/机器上启动所有Master组件，并且不会在此VM/机器上运行用户容器。请参考构建高可用群集以来构建multi-master-VM。
- kube-apiserver
	- kube-apiserver用于暴露Kubernetes API。任何的资源请求/调用操作都是通过kube-apiserver提供的接口进行。请参阅构建高可用群集。
- ETCD
	- etcd是Kubernetes提供默认的存储系统，保存所有集群数据，使用时需要为etcd数据提供备份计划。
- kube-controller-manager
	- kube-controller-manager运行管理控制器，它们是集群中处理常规任务的后台线程。逻辑上，每个控制器是一个单独的进程，但为了降低复杂性，它们都被编译成单个二进制文件，并在单个进程中运行。
- 这些控制器包括：
	- 节点（Node）控制器。
	- 副本（Replication）控制器：负责维护系统中每个副本中的pod。
	- 端点（Endpoints）控制器：填充Endpoints对象（即连接Services&Pods）。
	- Service Account和Token控制器：为新的Namespace创建默认帐户访问API Token。
- cloud-controller-manager
	- 云控制器管理器负责与底层云提供商的平台交互。云控制器管理器是Kubernetes版本1.6中引入的，目前还是Alpha的功能。

- 云控制器管理器仅运行云提供商特定的（controller loops）控制器循环。可以通过将--cloud-providerflag设置为external启动kube-controller-manager ，来禁用控制器循环。

- cloud-controller-manager 具体功能：
	- 节点（Node）控制器
	- 路由（Route）控制器
	- Service控制器
	- 卷（Volume）控制器

- kube-scheduler
	- kube-scheduler监视新创建没有分配到Node的Pod，为Pod选择一个Node。
- 插件 addons
	- 插件（addon）是实现集群pod和Services功能的。Pod由Deployments，ReplicationController等进行管理。Namespace 插件对象是在kube-system Namespace中创建。
- DNS
	- 虽然不严格要求使用插件，但Kubernetes集群都应该具有集群 DNS。
	- 群集 DNS是一个DNS服务器，能够为 Kubernetes services提供 DNS记录。
	- 由Kubernetes启动的容器自动将这个DNS服务器包含在他们的DNS searches中。
- 用户界面
	- kube-ui提供集群状态基础信息查看。
- 容器资源监测
	- 容器资源监控提供一个UI浏览监控数据。
- Cluster-level Logging
	- Cluster-level logging，负责保存容器日志，搜索/查看日志。
- 节点（Node）组件
	- 节点组件运行在Node，提供Kubernetes运行时环境，以及维护Pod。
- kubelet
	- kubelet是主要的节点代理，它会监视已分配给节点的pod，具体功能：
		- 安装Pod所需的volume。
		- 下载Pod的Secrets。
		- Pod中运行的 docker（或experimentally，rkt）容器。
		- 定期执行容器健康检查。
		- Reports the status of the pod back to the rest of the system, by creating amirror podif necessary.
		- Reports the status of the node back to the rest of the system.
- kube-proxy
	- kube-proxy通过在主机上维护网络规则并执行连接转发来实现Kubernetes服务抽象。
- docker
	- docker用于运行容器。
- RKT
	- rkt运行容器，作为docker工具的替代方案。
- supervisord
	- supervisord是一个轻量级的监控系统，用于保障kubelet和docker运行。
- fluentd
	- fluentd是一个守护进程，可提供cluster-level logging.。



