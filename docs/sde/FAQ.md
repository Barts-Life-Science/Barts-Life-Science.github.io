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

### I can't connect to my VM
Guacamole can be a bit flaky, and it sometimes takes a few goes to be able to connect to your VM. Here are a few things to try, in this order:
* First, if it's been more than an hour since you used your VM, go to the **Actions** menu and **Start** it. It may have been shut down automatically for having been idle. Wait 5 minutes, then try again.
* If that doesn't work, and Guacamole goes into a loop of retrying to connect every 15 seconds, try hitting the **Logout** button it shows at that time, then **Re-login** again.
* The next thing to try is the **Home** button on the Guacamole connection screen, and pick out the connection for your machine from there.
* Then try opening a new incognito window in your browser and try again.
* If that all fails, you can always destroy the VM and create a new one, assuming you haven't customised the VM too much.
* If you don't want to destroy the VM, [file a support ticket](https://github.com/Barts-Life-Science/Support), giving [all the relevant information](#how-do-i-get-help)