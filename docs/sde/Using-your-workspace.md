# Using your workspace

You access your workspace via the web, once you've been assigned an appropriate role. You will need to sign in with two-factor authentication, but you can access the workspace from anywhere, you do not have to be on the Barts Health VPN.

A workspace consists of several **workspace services**, which are specific to the workspace, plus they can also access **shared services**, which are shared by all the workspaces in the SDE. A **Workspace Owner** can create **workspace services**, and can manage all the resources in their own workspaces, regardless of who created them, but cannot manipulate the shared services. A **Workspace Researcher** can also create and use certain resources, but can only delete or modify resources they have created themselves.

## Getting access to the SDE

As of December 2024, the MVP release of the SDE is now available, and will be the default mode of providing data and compute resources to projects approved through the DAC. If you have been given access to it, you can log in at https://sde.bartshealth.nhs.uk/, using your credentials. These credentials will either be:
* Your usual Barts Health credentials that you would use to log into your laptop or VDI. Generally your desktop login (e.g.,  SmithJ@bartshealthnhs.uk)
* Dedicated credentials to access Precision Medicine related services (e.g., SmithJ@BHPrecisionMedicine.onmicrosoft.com). These will be provided if you do not have an NHS account that can be mapped into our Entra ID directory.

## Resource hierarchy: SDE, Workspace, User
Within the SDE, resources for a project are grouped into a Workspace, which has workspace-level resources. These workspace-level resources may also have user-level resources. The terms **project** and **workspace** both refer to the same thing as far as the SDE is concerned. From within a workspace, you will not have access to any other workspaces or their resources - this is a key feature of the SDE architecture. If you have multiple workspaces available to you, by way of being on multiple projects at the same time, their will be no direct connectivity between them.

Some examples of workspace services:
* **[Apache Guacamole](ttps://guacamole.apache.org/)** is a virtual desktop management system. This allows you to create virtual machines (Linux or Windows), and then get a virtual desktop to access them. Guacamole itself is a project-level resource, so you only need one per workspace, but the VMs created in it are specific to the user, so they are a user-level resource, not shared among multiple users.
* **[MySQL](https://www.mysql.com)** is a standard SQL database service, which, again, probably only needs one deployment per workspace. Each MySQL service can hold multiple databases, each with their own permissions.
* **[Gitea](https://about.gitea.com/)** is a git repository management system, similar to github. The SDE does not allow you to use github, since this would represent a back door for infiltrating/exfiltrating data, so if you want github-like functionality, you will have to install **Gitea** in your workspace instead, and host your code repositories there. You can import your initial codebase [through the airlock](Working-with-data.md), and any changes you make in these git repositories will need to be exported through the airlock to be used elsewere.
* **Azure Databricks** and **AzureML** are also available for machine-learning projects.

At the SDE level, the most important shared services are:
* **Nexus**. This is a [package mirroring repository from Sonatype](https://help.sonatype.com/en/sonatype-nexus-repository.html), which allows you to download packages from [CRAN](https://cran.r-project.org/), [PyPi](https://pypi.org/), [Anaconda](https://www.anaconda.com,) Ubuntu mirrors, and other sources. If you wish to access a package repository that is not currently mirrored then please raise a [support ticket](https://github.com/Barts-Life-Science/Support).
* **Gitea**. This shared service is useful for mirroring external Git repositories into the SDE network space so you can access them from within a workspace. Without this, you have no external access to, for example, GitHub. The access is one way, you cannot use this service to push changes back to GitHub. Access to this service can be provided by raising a [support issue](https://github.com/Barts-Life-Science/Support) and identifying the GitHub repositories that need to be mirrored.

There are other services available, and new services can be created by building templates for them. If you have a need for a service that's not currently represented, [contact us](https://github.com/Barts-Life-Science/Support).

## Accessing a workspace
When you log into the SDE, your initial view will look something like this. There will be a separate workspace for each project you have access to. In this example, there's only one project.
![Empty workspace](../assets/sde/00-empty-workspace.png)

Click on the workspace to get the **Workspace Overview** page. There are other tabs there you can explore, to get more information about your workspace. They're not normally useful during operation, but if you file a bug report or an issue, a screenshot from the **Operations** tab is often helpful.
![Workspace overview](../assets/sde/01-workspace-overview.png)

The workspace currently has no services in it. **Workspace Owners** can use the **Create new** button on the right to bring up a menu of resources to choose from, then fill in the form with a few parameters, and submit the form. Your workspace resources will then appear here, as you can see in the next screenshot. Here, we see a **Gitea** service, which is still deploying, and a **Guacamole** service, which is ready for use.

People with the **Workspace Researcher** role will not be able to create workspace services, for them, the **Create** button is greyed out.

N.B. We don't show the full process for deploying **Gitea** or **Guacamole**, since we will do that for you as part of the setup of your workspace. In any case, the process is very similar to that for creating a virtual machine, which is explained in detail below.
![Workspace with services](../assets/sde/03-workspace-with-services.png)

## Creating a Virtual Machine
Virtual machines are created via the **Guacamole** workspace service. From your workspace overview, click on the Guacamole service tile, which takes you to the screen below. Any existing VMs that you have access to will also be listed here, there are none at the moment.
![Virtual desktop service](../assets/sde/04-Virtual-Desktop-service.png)

Click the **Create new** button, then select an operating system, Windows or Linux.
![Choose a VM type](../assets/sde/05-choose-vm-type.png)

The next form is practically identical for Windows or Linux machines, only the dropdown options vary. Fill in the obligatory name and description, select an **image** (which specifies which types of software the machine will have, what version of the operating system it runs etc), and a **size**, which specifies the number of CPUs and amount of memory.

You cannot, at this time, specify the size of the disk for the virtual machine. That may be added as a feature later on, [let us know](https://github.com/Barts-Life-Science/Support) if you need that capability. However, you would be better to use the **shared storage** instead if you can.

There are a range of options for VM sizes, from 2 CPUs with 4 GB RAM up to machines with 36 CPUs, 880 GB RAM, and an A10 GPU. We can add more sizes if you need them, [let us know](https://github.com/Barts-Life-Science/Support).

Leave the **Shared storage** button selected. This means your VM will have access to storage that exists at the workspace-level, so will persist if you delete your VM. If you only use the VM disk, you may run out of space, and you will lose everything on it when the VM is destroyed. There's more info on that in the [[Working with data]] page.
![Fill in the parameters](../assets/sde/06-fill-in-the-parameters.png)

Click the **Submit** button, and after a few seconds, you see something like the next screenshot. You can either click the **Go to resource** button, or just click directly on the VM tile in the **Resources** section, to follow the progress of the deployment.

The deployment progresses through several stages:
* **pending** means the deployment is queued, but hasn't yet started
* **deploying** means the resource is being constructed
* **running** means the deployment succeeded

Other states exist, such as **failed**, or **updating**, hopefully you won't see those.
![VM is being created](../assets/sde/07-vm-is-being-created.png)

The **Operations** tab shows you detail of the deployment steps. The overall progress of the deployment is visible in the top-right in this view. Other buttons are greyed out at this point, and will remain so until the deployment finishes. This typically takes 5-10 minutes, but can take longer sometimes.

![Follow VM creation process](../assets/sde/08-follow-vm-creation.png)

![VM is there](../assets/sde/09-vm-is-there.png)

Now the VM is fully deployed, it's shown as **running** in the title, and the **Update**, **Delete** and **Actions** buttons are now active. In the **Actions** menu, there are three options:
* **Reset password** - ignore this, you don't need a password to access the VM.
* **Start** will start the VM if it has been stopped, and
* **Stop** will stop a running VM.

You will be charged for your VM all the time it is running. If you know you won't need it for a while (e.g. overnight, weekends, or holidays), it makes sense to **Stop** it when you don't need it, and **Start** it again to save your project budget. We have recently adding functionality to auto-shutdown machines which are idle for an hour, so you don't have to remember this for yourself, but it's still useful to know.

![VM action buttons](../assets/sde/10-vm-action-buttons.png)

Once the VM is fully booted, the Guacamole page will look like this. The VM is shown, with a **Connect** button, which will launch a new browser window connected to your VM. **Nota Bene:** The **Connect** button appears immediately the operating system has indicated the machine is up, but it can still take a few minutes doing post-boot configuration, so you may not be able to connect to it immediately. If you still can't connect after 10 minutes ot trying, [file a support ticket](https://github.com/Barts-Life-Science/Support)

![Guacamole with booted VM](../assets/sde/11-guacamole-with-booted-vm.png)

Clicking the **Connect** button takes you to your virtual desktop. In this case, it's Ubuntu. You can hover over the icons for help on each.

![Ubuntu Desktop](../assets/sde/12-ubuntu-desktop.png)

Clicking the **Terminal** icon brings up a terminal window. When you're finished with your session, just close the browser tab. Reconnecting later will bring it back in the same state.

![Ubuntu Desktop with terminal](../assets/sde/13-ubuntu-desktop-with-terminal.png)

## Working with Linux VMs
* Your Linux VM has no direct connectivity to the internet. If you need to update a package, or install new software, you can access various mirrors courtesy of the Nexus shared service, but you won't be able to download software from arbitrary sites.
* VMs are not backed up. If you delete a VM, everything on the disk is destroyed, with no possibility of recovering it.
* On the other hand, stopping and restarting the VM from the **Action** menu does not wipe the contents of your home directory, so is a safe way of saving money when you don't need the VM for a while. *However*, the **/tmp** directory *is* wiped on reboots, so don't store anything there if you want to keep it.
* The home directory is mounted on the root filesystem, which is not large. If you fill the disk completely, your VM may stop responding, and you may lose access to it, possibly losing your work in the process. For this reason, it's better to work with the shared storage which comes with your workspace.
* We provide a default amount of shared storage for every project, 500 GB currently. This can be increased at short notice, so if you find your shared storage fills up, [let us know](https://github.com/Barts-Life-Science/Support) and we can increase it for you.

## More details about workspaces
### Keeping VMs up to date
You should make sure you update the OS and software on your VMs regularly. Even though they're contained within a firewall, it's good practice to make sure you're not running old software. For this reason, we'll be updating the VM templates regularly, and you can take advantage of that by simply deleting your VM and creating a new one. We'll announce new VM template releases by email, probably quarterly.

If you find that you need to put a lot of effort into customising an image before you can use it, [let us know](https://github.com/Barts-Life-Science/Support), we may be able to do this for you, and build it into future image versions.

## Considerations for workspace managers

## Cost efficiency
Workspace Admins are responsible for managing the cost of their workspaces. Projects are expected to pay for their consumption of Azure resources, which is separate from the [one-time setup fees for a project in the Barts Health Data Platform](https://data.bartshealth.nhs.uk/Platform-Information/Charges/). In fact, the last entry in the table on that page is what we're talking about here.

There are basic, irreducible costs for the workspace to exist, amounting to about £3-5/day. Beyond that, each extra service or resource adds to the cost. In practice, VMs are the most expensive item, the shared-storage is much cheaper. Services like MySQL or AzureSQL which deploy specialised resources can also cost several pounds per day.

The cost of VMs varies from about £50/month for the smallest VM size to about £3500/month for the largest size with a GPU. That's assuming it runs 24x7 for the whole month. If it's shut down (Stopped) when not in use, it doesn't cost you anything for that time. So even just using it 40 hours per week already reduces the cost by two thirds. You can also keep costs down by developing your code on the shared storage using a small machine, then only booting up a larger machine if/when you need the extra horsepower for an analysis, having tested first on a smaller subset in the smaller VM.

Windows VMs cost slightly more than Linux VMs too, because of the Windows licence.

You can see the cost of your resources on the tile for that resource, which shows the actual cost for that resource and any sub-resources for the current month - i.e. it restarts from zero at the beginning of every month.

Most resources, and workspaces themselves, can be 'disabled', to save costs. Services that are disabled are hibernated, to the extent possible for that service, to minimise expenditure. You will still be charged for any storage used by a service during hibernation, but not for compute etc. Storage is generally cheap, so its can be ignored for most purposes. However, there is one very important exception to this rule. Disabling a VM does *not* shut it down and it will still incur costs. To stop paying for a VM, you have to either delete it completely, or **Stop** it from the **Actions** menu.

We recommend that you monitor costs of your services in the first days of accessing your workspace, and decide which services to disable/re-enable based on your budget and convenience.

We can provide a daily email with a breakdown of costs for your workspace for the last 30 days. This allows you to track the expenditure and see what you're spending it on. We will ask you who needs to receive these emails as part of the project setup process.