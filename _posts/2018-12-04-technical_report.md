---
layout: post

title: "Technical Report for CS 6421 Distributed Systems"

date: 2018-12-09

description: "conclude the topics I've learned about this semester"

tags: Course


---

In this semester, I toke the course CS 6421 Distributed Systems in George Washington University. As the following picture shows,  I learned the intro, network programming, servers and virtualization, networks, storage and fault tolerance, ordering and consistency, and scalable web service and serverless of distributed systems. Besides, I also learned some basic usages and their interactions of AWS technologies such as EC2, Lmabda, Elastic Beanstalk, and Amazon SageMaker. 

![lecture_review](/blog/images/posts/2018-12-09-Technical_Report_for_CS_6421_Distributed_Systems/lecture_review.png)

In this technical report, I will review the topic of **Cloud Computer: Servers and Virtualization**, which is very practical in software developing and configuration. 

### Cloud services

The most famous cloud computing service is provided by AWS (Amazon Web services). In 2016, Amazon had 68 total data centers in world and 50k-80k serverws per data center. The reason we use cloud is that it's cheap. People can pay for the services like compute power, database storage, content delivery and other functionality to help business grow with lower cost. For many small business, it provides a pay-as-you-go service, it's easy to scale, and people don't need to worry about many IT issuses.

There are different types of cloud services: Software as a Service (Saas), Platform as a Service (Paas), and Infrastructure as a Service (Iaas). Before this semester, I always confused these three topics, but now I have a clear minds.

The first service is **Saas**, also known as cloud application services, provides direct services to user via the internet. Most of the Saas applications are run through the browser, and don't need any downloads on the client side, like gmail and dropbox. The common examples of Saas are Google Apps like gmail, Dropbox, and GoToMeeting.

The second service is PaaS. PaaS is similar to Saas, except instead of devivering the software over the internet, PaaS provides a platform for software building. The software developers may like to use PaaS because it gives developers the freedom to concentrate on building the software while still not need to worry about operation systems, software updates, storage, or infrastructure. PaaS is built on virtualization technology which will be mentioned on next section. The common examples of PaaS are Google App Engine, Windows Azure, and AWS Elastic Beanstalk.

The last one is IaaS. IaaS provides cloud computing infrastructures to people, including things such as servers, networks, operating systems, and storage. It's just like you rent these devices to make your business. When you are a startup company, IaaS is a good option because you don't have to spend the time and money trying to build the hardware and software. The common examples of IaaS are AWS, Google Compute Engine, and Microsoft Azure. In addition, I want to talk about Amazon EC2 (Amazon Elastic Compute Cloud). In the previous practices, I use EC2 many times. In simple terms, Amazon EC2 provides the instances on which users set up and configure the operating system and applications. For normal use, EC2 is much cheaper than buying a server.

### Virtualization

When I was a freshman in university, I have used the virtual machine service -- VMware to run Linux system on my Windows PC. In this semester, I have a full understanding of virtualization. In one word, virtualization lets you run more applications on fewer physical servers. Through virtualization each app and operating system live in a separate software container called a virtual machine or VM. VMs are completed isolated but computing resources like CPUs, storage, and networking are pooled together and deliver dynamically to each VM by a software layer called **hypervisor**. Every application gets what it needs for peak performance with all your servers running at full capacity. The following picture shows the basic architecture of virtualization clearly: 

![virtualization](/blog/images/posts/2018-12-09-Technical_Report_for_CS_6421_Distributed_Systems/virtualization.png)

The virtualization have different types according to the virtualization degree. From top to bottom, they are:

- Application Virtualization

  - Runs special application code

  - Example: Java virtual machine (JVM)

- Hosted Virtualization

  - Virtualizes a full OS and apps

  - Example: VMware Player (I have used it to run Linux on my Windows PC)

- Paravirtualization

  - Modify OS to simply hypervisor

  - Example: Xen (Xen project is a hypervisor that allows multiple computer operating systems to execute on the same computer hardware concurrently)

- Full Virtualization

  - Runs directly on HW

  - Example: VMware ESXi (includes and integrates vital OS components, such as a kernel)

### Container

Over the last decade, the virtualization has grown in popularity to enable software to run predictably and well when moved from one server environment to another. But **containers** provide a way to run these isolated systems on a single server or host OS.

Containers sit on top of a physical server and its host OS—for example, Linux or Windows. Each container shares the host OS kernel and the binaries and libraries, too. Shared components are read-only. Containers are thus very “light”—they are only megabytes in size and take just seconds to start, versus gigabytes and minutes for a VM. Containers also reduce management overhead. Because they share a common operating system, only a single operating system needs care and feeding for bug fixes, patches, and so on. This concept is similar to what we experience with hypervisor hosts: fewer management points but slightly higher fault domain. In short, containers are lighter weight and more portable than VMs. The following picture shows the architectures of Virtualization and Container:

![virtualization_container](/blog/images/posts/2018-12-09-Technical_Report_for_CS_6421_Distributed_Systems/virtualization_container.png)

Virtual machines and Containers differ in several ways, but the primary difference is that containers provide a way to virtualize an OS so that multiple workloads can run on a single OS instance. With VMs, the hardware is being virtualized to run multiple OS instances. Containers’ speed, agility, and portability make them yet another tool to help streamline software development.

In the practice of *Docker and Containers*, I have tried to use Docker which is a typical container by myself. For example, I execute the script `docker run hello-world` which means run the container named *hello-world*. Enssentially, the Docker engine running in my terminal tried to find an image named hello-world, if there are no images stored locally, the Docker engine goes to its default Docker register, which is**Docker Store**, to look for an image named*hello-world*. It finds the image there, pulls it down, and then runs it in a container. Another example is I ran several commands via container instances with the help of`docker container run`. The `docker container ls -a` command showed us that there were several containers listed. Why are there so many containers listed if they are all from the alpine image? This is a critical security concept in the world of Docker containers. Even though each docker container run command used the same alpine image, each execution was a separate, isolated container. Each container has a separate filesystem and runs in a different namespace; by default a container has no way of interacting with other containers, even those from the same image.

### Conclusion

In this technical report, I mainly talk about the topic of cloud service, virtualization, and container. These cloud technologies are becoming the powerful tools for us developers to build applications or manage the systems. In the following semester, I'll concentrate on mastering different AWS to help me build my own software.
