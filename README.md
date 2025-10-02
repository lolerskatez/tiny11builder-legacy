# tiny11builder
*Scripts to build a trimmed-down Windows 11 image - now in **PowerShell**!*

## Introduction :
Tiny11 builder, now completely overhauled. <br> After more than a year (for which I am so sorry) of no updates, tiny11 builder is now a much more complete and flexible solution - one script fits all. Also, it is a steppingstone for an even more fleshed-out solution.

You can now use it on ANY Windows 11 release (not just a specific build), as well as ANY language or architecture.
This is made possible thanks to the much-improved scripting capabilities of PowerShell, compared to the older Batch release.

This is a script created to automate the build of a streamlined Windows 11 image, similar to tiny10.
The script has also been updated to use DISM's recovery compression, resulting in a much smaller final ISO size, and no utilities from external sources. The only other executable included is **oscdimg.exe**, which is provided in the Windows ADK and it is used to create bootable ISO images. 
Also included is an unattended answer file, which is used to bypass the Microsoft Account on OOBE and to deploy the image with the `/compact` flag.
It's open-source, **so feel free to add or remove anything you want!** Feedback is also much appreciated.

Also, for the very first time, **introducing tiny11 core builder**! A more powerful script, designed for a quick and dirty development testbed. Just the bare minimum, none of the fluff. 
This script generates a significantly reduced Windows 11 image. However, **it's not suitable for regular use due to its lack of serviceability - you can't add languages, updates, or features post-creation**. tiny11 Core is not a full Windows 11 substitute but a rapid testing or development tool, potentially useful for VM environments.

---

## ⚠️ Script versions:
- **tiny11maker.ps1** : The regular script, which removes a lot of bloat but keeps the system serviceable. You can add languages, updates, and features post-creation. This is the recommended script for regular use. **Now with both modern and legacy installer support!**
- ⚠️ **tiny11coremaker.ps1** : The core script, which removes even more bloat but also removes the ability to service the image. You cannot add languages, updates, or features post-creation. This is recommended for quick testing or development use.

## Instructions:
1. Download Windows 11 from the [Microsoft website](https://www.microsoft.com/software-download/windows11) or [Rufus](https://github.com/pbatard/rufus)
2. Mount the downloaded ISO image using Windows Explorer.
3. Open **PowerShell 5.1** as Administrator. 
5. Change the script execution policy :
```powershell
Set-ExecutionPolicy Bypass -Scope Process
```
> Using `-Scope Process` you keep your original policy intact as this change only lasts for the current PowerShell session. 

6. Start the script :
```powershell
C:/path/to/your/tiny11/script.ps1 -ISO <letter> -SCRATCH <letter>
``` 
> For the legacy installer format, you can add the `-Legacy` parameter or select it interactively during the script execution.
> You can see full details of the script by running the `get-help` command.

6. Select the drive letter where the image is mounted (only the letter, no colon (:))
7. Select the SKU that you want the image to be based on.
8. Choose between modern (Windows 11 style) or legacy (Windows 10 style) installer.
9. Sit back and relax :)
10. When the image is completed, you will see it in the folder where the script was extracted, with the name `tiny11.iso` (for modern installer) or `tiny11_legacy.iso` (for legacy installer)

---

## What is removed:
<table>
  <tbody>
    <tr>
      <th>Tiny11maker</th>
      <th>Tiny11coremaker</th>
    </tr>
    <tr>
      <td>
        <ul>
          <li>Clipchamp</li>
          <li>News</li>
          <li>Weather</li>
          <li>Xbox</li>
          <li>GetHelp</li>
          <li>GetStarted</li>
          <li>Office Hub</li>
          <li>Solitaire</li>
          <li>PeopleApp</li>
          <li>PowerAutomate</li>
          <li>ToDo</li>
          <li>Alarms</li>
          <li>Mail and Calendar</li>
          <li>Feedback Hub</li>
          <li>Maps</li>
          <li>Sound Recorder</li>
          <li>Your Phone</li>
          <li>Media Player</li>
          <li>QuickAssist</li>
          <li>Internet Explorer</li>
          <li>Tablet PC Math</li>
          <li>Edge</li>
          <li>OneDrive</li>
        </ul>
      </td>
      <td>
        <ul>
          <li>all from regular tiny +</li>
          <li>Windows Component Store (WinSxS)</li>
          <li>Windows Defender (only disabled, can be enabled back if needed)</li>
          <li>Windows Update (wouldn't work without WinSxS, enabling it would put the system in a state of failure)</li>
          <li>WinRE</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

Keep in mind that **you cannot add back features in tiny11 core**! <br>
You will be asked during image creation if you want to enable .net 3.5 support!

---

## Installer Types:

The script now supports two different installer formats:

1. **Modern Installer (Windows 11 style)**
   - Default option if not specified with parameters
   - Uses the newer Windows 11 installation experience
   - Creates `tiny11.iso`

2. **Legacy Installer (Windows 10 style)**
   - Can be specified using the `-Legacy` parameter or selected interactively
   - Uses the classic Windows 10 installation experience
   - May be preferred for compatibility with certain hardware or deployment scenarios
   - Creates `tiny11_legacy.iso`

You can select your preferred installer type either:
- By adding the `-Legacy` parameter when running the script
- Interactively during script execution after selecting the Windows image

---

## Known issues:
- Although Edge is removed, there are some remnants in the Settings, but the app in itself is deleted.
- You might have to update Winget before being able to install any apps, using Microsoft Store.
- Outlook and Dev Home might reappear after some time. This is an ongoing battle, though the latest script update tries to prevent this more aggressively.
- If you are using this script on arm64, you might see a glimpse of an error while running the script. This is caused by the fact that the arm64 image doesn't have OneDriveSetup.exe included in the System32 folder.
- ~~In legacy installer mode, drivers may be missing requiring manual loading of SATA and other drivers~~ (Fixed in the 10-02-25 update - drivers are now preserved in legacy installer mode)

---

## New in this release:
- **Legacy installer support**: Create ISOs with Windows 10-style installer format
- **Interactive installer selection**: Choose between modern and legacy installer during script execution
- **Automatic architecture detection**: Script properly handles different CPU architectures

## Features to be implemented:
- ~~disabling telemetry~~ (Implemented in the 04-29-24 release!)
- ~~more ad suppression~~ (Partially implemented in the 09-06-25 release!)
- ~~legacy installer support~~ (Implemented in the 10-02-25 release!)
- improved language and arch detection
- more flexibility in what to keep and what to delete
- maybe a GUI???

And that's pretty much it for now!
## ❤️ Support the Project

If this project has helped you, please consider showing your support! A small donation helps me dedicate more time to projects like this.
Thank you!

**[Patreon](http://patreon.com/ntdev) | [PayPal](http://paypal.me/ntdev2) | [Ko-fi](http://ko-fi.com/ntdev)**
Thanks for trying it and let me know how you like it!
