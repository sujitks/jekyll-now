---
layout: post
---

## Windows 8 Developers Preview

Alright so next version of windows (windows8) is coming soon now. The moment I got the news that a developer preview of windows 8 is available for the download, I was very excited to see this running. I planned to download this and install in my ESXI infrastructure so that this could be tested/used optionally without making any physical machine a brick.
![screenshot](/images/post1-300x195.png "Screenshot")
I first tried to install this directly in ESXI but no success. It gave me HAL INTIALIZATION ERROR, which turned out to be known issue reported by many of folks over the internet. Instead of spending time to do more research and try to understand the issue, I thought to install it on other virtualization system. VMware client gives different error stating something is missing and a long technical log.

I then thought to give Virtual Box a try. Since a long time, I was planning to try Virtual Box so this seemed to me a good opportunity to experience Virtual Box as well as Windows8.

Installation of Virtual Box was an easy task, got this downloaded from www.virtualbox.org. It has got installed on my new laptop without any issue or much to configure. Very easy and nice installation wizard I must say.

Once Virtual Box has been installed, new virtual machine wizard started.

New virtual machine wizardIn the next screen wizard asks for the operating system to be installed, I have selected windows 7 64 bit as the operating system as Windows8 was not there. I was assuming that if windows8 would be based on any other MS OS, it is more likely to be based on Windows7. My next choice was to select windows 2008 in case windows 7 options would not work.

After that virtual machine wizard asks for the memory to allocate. I have selected 1GB for it. These options are typically same as any other virtual machine creation either on the VMware infrastructure or MS Virtual machine infrastructure. I have not experienced Hyper V yet but assuming this would be same there as well.

You can expect typical configuration option while creating a virtual machine. Machine are machine regardless actual or virtual.

Next option was selection of a hard disk. This could be either a new hard disk which would be created by virtual box for you, or an  Virtual Machine wizard, Hard Diskexisting hard disk could be located, if you have one created.

Virtual Box seems to support number of virtual disk formats. I am not sure whether hard drive created by other vm infrastructure could be used or not. I would cover this in another post to see compatibility between different VM infrastructures produced resources.

I have selected a new VM disk and selected VMDK option, I thought once VM is created this might be used in my ESXI by exporting there. I was wrong there. After installation of Windows 8 in a Virtual Box environment, I did try to export the machine to my ESXI infrastructure which was successful, however when open from ESXI HAL INITIALIZATION ERROR came back again. :(

You can choose size allocated to hard disk by specifying the actual size or choose for dynamic size option. Dynamic size option would allow disk to grow as and when extra space needed.

Once sizing is selected it shows summary screen to remind all the option selected, should you want to change anything, and it can be done before clicking the ‘Create’ button.

Summary page of Virtual Machine creation wizard

Click on ‘Create’ and your virtual machine is ready for OS installation. After this you would have to power on the newly created virtual machine from the Oracle Virtual Machine manager. After switching on the power of the machine, downloaded ISO of Windows 8 Developer Preview installation could be mounted to CD/DVD drive.

Once these steps are done, restart your virtual machine and setup files in the ISO will guide you for the installation of the Developer Preview. See steps carried out during installation of the OS.Installation StepsOnce the installation steps are done, System restarts few times (2 times I think), and License page gets displayed. After accepting the license it moves further to setup the system.

License Terms
License Terms - Windoes 8 Developer Preview

Most of the screens in the system gives same experience as Windows Phone 7. See the setting screen for configuring the Windows, all the options are presented in the same way as they are on the device.

Settings
Settings - Windows 8 Developers Preview

User Account
User Account - Windws 8 Developers Preview

After setting up the basic configuration in settings option, Windows asks for the credentials of a live account. Yes, it does asks for the credentials of a Live account which means this OS need to communicate with internet and access your profile. I am sure there would be options in the other flavour of this OS where you can create and manage local accounts, However for Developers Preview it does asks for the Live credentials.

I have uploaded a video to demonstrate installation of Windows 8 Developers Preview which could be found at “Windows 8 Developer Preview – First Look”.

These are all the steps you would need to start experiencing Windows 8 Developers Preview edition. I have not tested many of its options but it looks cool. I am sure that for rest of the weekend and next week I would be using this to explore further. It has Visual Studio 11 Express pre-installed. Expression Blend 5 is also pre-installed.

Home
Home - Windows 8 Developers Preview

After using just for few minutes, I found new operating system (Windows 8) is a fun to use. User experience is very intuitive and layout is so cool.

I would also mention here that many things are provided considering the usages of apps these days. This looks like a practical Web based operating system which is connected with your live account. Many people who have prior experience of using windows phone 7 would get this very familiar. Many live tiles looks same as your windows phone7, however we know this is a full blown operating system for PC and Laptops.

I would not be surprised to get the news that very soon there would be many slick tablets coming with this OS.



Another thing which I noticed is that it looks very fast and responsive. I know there are not many things installed and running, however considering that it is running inside a virtual machine with limited and shared resources, performance is nice so far.

As always, I would appreciate your comments for the post till I would explore it further.
Thanks
