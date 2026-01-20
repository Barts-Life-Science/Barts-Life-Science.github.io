# Frequently Answered Questions

## Support
### How do I get help?
The preferred channel is that you [file a support ticket](https://github.com/Barts-Life-Science/Support) in our github account which is specially for this purpose. Please give as much information as you can, it will save us time having to ask you for it. For example:
* When you see the issue?
* What project are you in?
* What's the name of the VM you're using, or whatever other resource shows the problem?
* Are there any steps we can follow to reproduce the problem?
* Have you tried reproducing the problem yourself on other VMs?
* Is this problem blocking you completely, or do you have a workaround?
* Include screenshots of the issue if you can

## Virtual Machines
### What software is installed on the Virtual Machines?
We're aiming for consistency between the flavours of Windows and Linux, so they should eventually (Easter 2025) have the same versions of everything installed on all OS flavours. We're not quite there yet, but we're close. The software we're curating is:

* Python, Conda, Jupyter, R, RStudio, Git
* Docker
* Azure CLI, Azure Storage Explorer, Azure Data Studio, AZCopy
* Visual Studio Code
* Chrome, Firefox, Microsoft Edge
* Libreoffice
  * We can't install Microsoft Office tools yet because of licensing. We may be able to solve that eventually, but even then, for access on Linux, Libreoffice provides compatibility
* micro DICOM, Radiant, Spyder.
  * These are DICOM viewers. Radiant is Windows-only
* MySQL, MySQL workbench, Postgresql, Dbeaver.
  * Dbeaver is a DB management tool that can be used with several SQL DB flavours.
  * Note also that there is a MySQL service and an AzureSQL service which are better options than hosting a SQL DB in your VM, for many reasons.

### How much will my VM cost?
VM prices vary considerably, depending on the number of CPUs and the amount of memory. The VM flavours we currently offer vary in price from about £60/month for a small Linux machine to over £4000/month for the largest GPU flavour running Windows, see the table for more details.

Machines with GPUs cost a lot more than machines with only normal CPUs. There are many other flavours of GPU available in Azure, we can add them to the SDE if you need them.

Windows VMs cost more than Linux VMs because of the Windows license. We try to maintain the same set of software on both platforms, so if you can use Linux, you may be able to save money too.

A VM only incurs cost while it's running. If you **Stop** the VM, it will not cost you anything until you **Start** it again. So if you only use a VM during office hours, you can expect it to cost 1/3 of the price shown below.

**N.B.** The **Stop** and **Start** actions are under the **Actions** menu for each VM. The **Disable** and **Enable** buttons *do not* power down the VM, so *do not save you money*. When you Stop your VM it will show as **deallocated** in the SDE workspace.

Type name | #CPUs | RAM (GB) | #GPUs | Linux (£/month) | Windows (£/month)
| :--- | ---: | ---: | :---: | ---: | ---: |
Standard_D2s_v5 | 2 | 8 | - | 65 | 120
Standard_D4s_v5 | 4 | 16 | - | 130 | 240
Standard_D8s_v5 | 8 | 32 | - | 260 | 480
Standard_D16s_v5 | 16 | 64 | - | 520 | 950
Standard_D32s_v5 | 32 | 128 | - | 1000 | 1900
Standard_D64s_v5 | 64 | 256 | - | 2100 | 3800
Standard_NV6ads_A10_v5 | 6 | 55 | 1/6 x A10 | 330 | 490
Standard_NV12ads_A10_v5 | 12 | 110 | 1/3 x A10 | 670 | 990
Standard_NV18ads_A10_v5 | 18 | 220 | 1/2 x A10 | 1200 | 1700
Standard_NV36ads_A10_v5 | 36 | 440 | 1 x A10 | 2300 | 3300
Standard_NV36adms_A10_v5 | 36 | 880 | 1 x A10 | 3300 | 4300

### What will the rest of my workspace cost?
Charges for Azure components in the SDE come down to one of three categories:
* Fixed-cost infrastructure, such as network components and application gateways so you can connect to VMs. These are typically £3 - £5 per day, and cannot be reduced without a lot of engineering work.
* Charges for compute elements. This means Virtual Machines, but also compute clusters for Azure ML, possibly a few other services. See above for compute element cost.
* Charges for storage. This means shared storage, as well as any database storage. This is cheap, compared to computing, the shared storage will cost about £75/month for one TB. With storage, you can expect to only pay for what you use, so unless you're in the TB range, it's very likely that your computing costs will dwarf your storage costs.

### I can't connect to my VM
Guacamole can be a bit flaky, and it sometimes takes a few goes to be able to connect to your VM. Here are a few things to try, in this order:
* First, if it's been more than an hour since you used your VM, go to the **Actions** menu and **Start** it. It may have been shut down automatically for having been idle. Wait 5 minutes, then try again.
* If that doesn't work, and Guacamole goes into a loop of retrying to connect every 15 seconds, try hitting the **Logout** button it shows at that time, then **Re-login** again.
* The next thing to try is the **Home** button on the Guacamole connection screen, and pick out the connection for your machine from there.
* Then try opening a new incognito window in your browser and try again.
* If that all fails, you can always destroy the VM and create a new one, assuming you haven't customised the VM too much.
* If you don't want to destroy the VM, [file a support ticket](https://github.com/Barts-Life-Science/Support), giving [all the relevant information](#how-do-i-get-help)

# Software on VMs
## R / RStudio
### How do I install R packages on Ubuntu?
There's [a bug in the way `R` and `RStudio` are configured on Ubuntu in the SDE](https://github.com/microsoft/AzureTRE/issues/4657) at the moment, which means they don't succeed in installing packages in the usual manner (e.g. `install.packages( "tidyverse" )` fails in both)

There are two ways to work around this. The first is to open a terminal, run `sudo bash` to become root, then edit `/etc/R/Rprofile`. You'll see an extra double-quote in the middle of the proxy string, just remove that and restart your R session.

That will allow you to install packages, compiling them from source.

If you want to install binary packages instead, for speed or reproducibility, you can do it another way, via conda:

N.B. If this is the first time you've use `conda` in your virtual machine, first run `conda init`, then exit your shell/terminal and start a new one.

```bash
conda create -n my-env -y
conda activate my-env
conda install -y -c conda-forge r-dbi r-odbc r-dbplyr r-tidyverse
export RSTUDIO_WHICH_R=$(which R)
rstudio
```

From then on, for every new terminal session, you can just:
```bash
conda activate my-env
export RSTUDIO_WHICH_R=$(which R)
rstudio
```

Once rstudio starts, you should be able to `require( tidyverse )` successfully.
