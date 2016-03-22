# Docker Hands-on Lab

This is a hands-on lab for getting started with Docker using Microsoft Azure.

## Lab Setup

Before you can perform any exercises in this lab, please follow the [Setup](Setup/README.md) section which will help you provision the required Azure resources.
The [Setup](Setup/README.md) section also lists requirements for this lab.

## Exercises in this lab

[Setup - Create a Free Azure Subscription and set up Ubuntu Docker Host](Setup/README.md)

[Exercise 1 - Docker Commands](Exercise01/README.md)

[Exercise 2 - Docker Images](Exercise02/README.md)

[Exercise 3 - Multi-container Applications with Docker Compose](Exercise03/README.md)

[Exercise 4 - Multi-host Cluster with Docker Swarm](Exercise04/README.md)

[Exercise 5 - Running ASP.NET 5 Web Applications under Linux with Docker](Exercise05/README.md)

## What is Docker?

[Docker](https://www.docker.com/) allows you to package an application with all of its dependencies into a standardized unit for software development.

![](images/what_is_layered_filesystems_sm.png)

Docker containers wrap up a piece of software in a complete filesystem that contains everything it needs to run: code, runtime, system tools, system libraries - anything you can install on a server. This guarantees that it will always run the same, regardless of the environment it is running in. 

Docker containers are **lightweight**. Containers running on a single machine all share the same operating system kernel so they start instantly and make more efficient use of RAM. Images are constructed from layered filesystems so they can share common files, making disk usage and image downloads much more efficient.

<table border="0" cellpadding="0" cellspacing="0">
    <tr>
        <td style="width: 50%;">
            <img src="images/what-is-docker-diagram.png"/>
        </td>
        <td style="width: 50%;">
            <img src="images/what-is-vm-diagram.png" style="margin-top: 154px;"/>
        </td>
    </tr> 
</table>

## What is Microsoft Azure?

[Microsoft Azure](https://azure.microsoft.com/) is a growing collection of [integrated cloud services](https://azure.microsoft.com/en-us/services/) - analytics, computing, database, mobile, networking, storage, and web.

Azure supports the broadest selection of operating systems, programming languages, frameworks, tools, databases and devices. Run Linux containers with Docker integration; build apps with JavaScript, Python, .NET, PHP, Java and Node.js; build back-ends for iOS, Android and Windows devices. Azure cloud service supports the same technologies millions of developers and IT professionals already rely on and trust.

### References

This hands-on lab borrows and adapts content from the following sources:

1. [The official Docker documentation](https://docs.docker.com/)
2. [The official Microsoft Azure documentation](https://azure.microsoft.com/en-us/documentation/)
3. [Running ASP.NET 5 applications in Linux Containers with Docker](https://blogs.msdn.microsoft.com/webdev/2015/01/14/running-asp-net-5-applications-in-linux-containers-with-docker/)
4. [Docker Swarm Cluster template](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-swarm-cluster)
5. [Docker Swarm Container Service Walkthrough](https://github.com/Azure/azure-quickstart-templates/blob/master/101-acs-swarm/docs/SwarmPreviewWalkthrough.md)
