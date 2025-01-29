# The Secure Data Environment

This page contains the user documentation for the Barts Health Secure Data Environment (SDE).

## Introduction

A note on terminology: We refer to a **Secure Data Environment** (SDE) and a **Trusted Research Environment** (TRE) interchangeably here. Locally, we prefer the SDE designation, but we're based on a package which calls itself a TRE. To all intents and purposes, they mean the same thing.

## What is a Secure Data Environment?

As the name implies, an SDE is an environment in which data can be analysed securely. This means that there is no direct access to the internet, data cannot be uploaded or downloaded freely by the User. This greatly reduces the possibility of data leaks, accidental or otherwise, and is a requirement for handling sensitive data.

Data can only be put into or extracted from the SDE by one of two routes:
* Data from Barts Health requested and approved via the [Data Access Committee (DAC)](https://bartslifesciences.org/precision-medicine/) will be pushed into the project storage after any appropriate pre-processing, such as anonymisation. With the approval of the DAC, we can link data sets from other Data Controllers to our own data either anonymised or psuedoanonymised, depending on the requirements.
* Other data, software or tools can be imported or exported by going through an []'airlock'](Working-with-data.md) process, where the data is staged through secure storage and is reviewed by the Precision Medicine team before access is granted and the material is made available the other side of the airlock.

In order to keep the data secure, there are some restrictions on the analysis environment. The data can be accessed through **virtual machines** (VMs), contained in a **workspace**. Each workspace is unique to a project, with only project members having access to it. The VMs in a workspace cannot be accessed directly through [Secure Shell (SSH)](https://www.openssh.com/), they have to be accessed by a web-based virtual desktop. Copy/paste activity is restricted, see the [airlock documentation](Working-with-data.md) for details.

[Our SDE](https://github.com/Barts-Life-Science/AzureTRE) is based on the [AzureTRE](https://github.com/microsoft/AzureTRE) package from Microsoft. This is an implementation built by Microsoft as an 'accelerator' for the community. It's not a product in the sense that you can't click on a button in the Azure portal and have it deployed, and they don't officially support it at this time. For this reason, we have to develop it further ourselves, so that it satisfies all our requirements. See the [timeline](#Timeline) below for more details.

## Project workspaces
A workspace is a container for resources, or 'services' for a project. Services include things like a Git mirror based on [Gitea](https://about.gitea.com/), a [Nexus repository](https://www.sonatype.com/products/sonatype-nexus-repository) which allows you to access software repositories such as [CRAN](https://cran.r-project.org/) and [PyPi](https://pypi.org/), and a [Guacamole Virtual Desktop](https://guacamole.apache.org/) service, which allows you to create and access virtual machines.

Once a project has been approved through the DAC, we will provision a workspace for it. Users will be assigned one of two roles:
* **Workspace Owner**, who can create and destroy resources in the workspace, or 
* **Workspace Researcher**, who can use resources, but can only create or destroy selected resources, such as VMs.

A Workspace Owner can manage all the resources created by any Workspace Researcher, but a Workspace Researcher can only manage resources that they themselves create.

You should then be able to [log into the SDE](https://sde.bartshealth.nhs.uk/), select your workspace, and view/manage/use the resources in your project.

## Getting help
We have further information on how you can provide feedback and get [support](https://github.com/Barts-Life-Science/Support).

## Using your workspace
See [Using your workspace](Using-your-workspace.md)

## Working with data
See [Working with data](Working-with-data.md)

## FAQ
See the [FAQ](FAQ.md)