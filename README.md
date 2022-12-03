# RunAsTrustedInstaller
<p align=center>
<img src=https://i.imgur.com/lTrx8hY.jpg>Screenshot</img>
</p>

## Run a program as TrustedInstaller (SYSTEM)

This is a twinBASIC x64-compatible port of my original VB6 project: [[VB6] Run process as TrustedInstalled (NT AUTHORITY\SYSTEM) w/ full system privileges)](https://www.vbforums.com/showthread.php?895287-VB6-Run-process-as-TrustedInstalled-(NT-AUTHORITY-SYSTEM)-w-full-system-privileges)

This version adds a couple features: tB TextBoxes support Unicode so you can now use the full UTF-8 range in the path, the plain textbox for inputting the path has been replaced with a ComboBox with MRU list, and a file picker has been added to browse for the target instead of manually typing it out (uses tbShellLib for this-- that's why the source is so large). 

===
If you've been running Windows 10, 11, and even to some extent 7, you've no doubt found even a so-called Administrator account doesn't have permission to do many things. Certain files, registry keys, etc, can only be accessed with the SYSTEM account under which many OS services run. TrustedInstaller is particularly privileged among system processes: It's the owner of many protected files and registry keys, so even just running as SYSTEM would leave you needing adjust permissions to take ownership (which you might not even be able to do) before altering them. So if you look at programs that do one of the most highly protected operations, disabling Windows Defender, they impersonate this process rather than simply run as SYSTEM (and why programs like AdvancedRun have 'Run as TrustedInstaller' as a separate option in addition to 'Run as SYSTEM').

This code shows you how to use the undocumented API NtImpersonateThread to have your thread impersonate TrustedInstaller, which allows full access to duplicate it's security token and start a process with the same privileges, running as the NT AUTHORITY\SYSTEM user. A big advantage of doing it this way is that we don't have to be running as a service, like the methods used by some other privilege escalating techniques.

Nirsoft's AdvancedRun has a feature to do this, but which sadly isn't open source much less written in VB, so I wanted to create something like it in VB6. I'll probably add more features like it has in the future, but this was the most complicated and useful.

Important: You must use Run As Administrator to use this code; it simply allows an admin to be an actual admin, it cannot escalate a unprivileged normal user to administrator. It does appear to work when run from the IDE, but that will of course need to also have been run with administrator privileges. 

### Usage

Arguments are supported. Paths containing spaces must be enclosed in quotes.
