Download [Windows 10](https://tb.rg-adguard.net/public.php).

Download [Arch Linux](https://archlinux.org/download/).


Open [Microsoft Windows 10 update history](https://support.microsoft.com/en-us/topic/windows-10-update-history-8127c2c6-6edf-4fdf-8b9f-0f7be1ef3562) and click on the lastest update/first from the top.
Open [Microsoft Update Catalog](https://www.catalog.update.microsoft.com/home.aspx). Search for the version of windows you have downloaded and add x64.

png1

Check if KB matches the one you are trying to download.

png2

As a last check, you can click on the update and verify on the package details tab that is says n/a and it's not superseeded by any other update.

png3

Download the scripts from this repository.

Create a bootable USB drive with [Ventoy](ventoy.net) and copy to it the above downloaded.

Restart and boot from USB. Disconnect from the internet, either by pulling out the ethernet cable, or by disabling from the bios. This will be required until we boot into linux.

Install windows.
During the initial post-install setup, select your default language, skip any network setup (it will ask twice, click no on the second prompt), and add a user as a local account. This user will be the user which does not have administrator privileges. The username can be anything you want, but on AME releases the username is simply “user”.

https://ameliorated.info/video/Windows_Initial_Startup.mp4

Simplifying the UI and removing extraneous visual features is one of the critical aspects of the amelioration process (as well as ensuring that certain subsequently damaged and/or non-responsive features are pulled from the interface). The following tasks need to be undertaken before the amelioration and ameliorate scripts are executed. Given the various extraneous and difficult to describe UI elements to be navigated for these procedures, videos have also been added to help document these basic tasks.

https://ameliorated.info/video/startmenu_taskbar.mp4

Before further changing the operating system, we recommend installing Microsoft’s security updates. Both the Servicing Stack Update (SSU) and Cumulative Updates are required to properly install updates. The Cumulative Update includes all updates released since the initial release of the version. This means that only the latest Cumulative Update is required to obtain updates included in prior Cumulative Updates. The lastest SSU package can be found in the KB.msu you downloaded earlier. 

png4

This update needs to be done before running the scripts.

Search for CMD, right click and open as administrator.

Extract the desired .cab file from the .msu files:

````powershell
C:\> expand -F:* <.msu file> <dest>
````
The SSU will need to be installed prior to the Cumulative Update, but the DISM command structure is identical. Just point to the correct .cab file.

Install SSU:
````powershell
C:\> dism /online /add-package /packagepath=C:\Path To\SSU-File.cab
````
Reboot before installing the Cumulative Update.

Install the Cumulative Update.
````powershell
C:\> dism /online /add-package /packagepath=C:\Path To\KB-File.cab
````
Reboot once more.
Lastly, clean the Windows Update cache:
````powershell
C:\> dism /online /Cleanup-Image /StartComponentCleanup
````
Sometimes the progress bar hangs in the command prompt; it will update if you type on the keyboard. Use the arrow keys since that will not put text on the screen. Cleaning up the cache currently takes longer than installing updates.

The Windows scripts can be run from any location whilst the bash script must be placed at the root of your windows installation (C:\).

Copy the two scripts to C:\

Right click and run as administrator amelioration_21H1_2021-10-13.bat

Once opened, run option 1 Pre-Amelioration from the main menu. This may take several minutes to complete, this will prompt for a reboot after completion. 

To assure that our changes are permanent, we need to remove Windows Update and its self-healing ability. This cannot be done within the running system because of Windows file permissions and repair operations.
For this step, we will use the Arch iso.

Reboot system and boot into the Live Linux environment. Enable the internet connection. From the terminal, do the following:
````bash
Install required packages.
# pacman -Sy ntfs3-g fzf
# lsblk
Mount the drive you’ve intalled windows on.
# mount /dev/sdaX /mnt
Chroot to the mounted device.
# arch-chroot /mnt
Make the script executable.
#chroot chmod +x ame-arch
Run the script.
#chroot ./ame-arch
````
After the script is done and you’ve rebooted back in windows run the amelioration_21H1_2021-10-13.bat as Administrator again.

[Optional] Run option 2 Post-Amelioration from the main menu. This will install some commonly used programs.

Run option 3: User Permissions, which will open the netplwiz GUI for configuring Windows user permissions:

Double click user account, go to the Group Membership tab and select Standard User. Click OK when finished. You will be asked to sign out, click Yes.
After logging back in, change the password of the Administrator.

In an elevated command prompt, type the following:
````powershell
C:\> net user Administrator *
````
Note that the password will not be showed on the screen.
