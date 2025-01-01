# PowerShell Execution policy

If we get errors such as the following when trying to check the node version:
```powershell
npm : File C:\Program Files\nodejs\npm.ps1 cannot be loaded. The file C:\Program Files\nodejs\npm.ps1 is not digitally signed. You cannot run this 
script on the current system. For more information about running scripts and setting execution policy, see about_Execution_Policies at 
https:/go.microsoft.com/fwlink/?LinkID=135170.
```

it is because we aren't authorized to execute these scripts. 

We can solve this by setting an execution policy that allows us to execute them.


NOTE:
As I learn more about Windows administration and permissions management, I may find reasons not to do the following, or find more suitable ways
to make things work. For now I will go with best effort.

We can see more on the topic at this webpage:
```https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-7.4```

For now, we can open powershell in admin mode and run the following to set execution policies to unrestricted:
```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted
```

Alternatively we can run this to allow the current user to execute commands like ```npm -v```:
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```


We can run the following to check the execution policies:
```powershell
Get-ExecutionPolicy -List
```
Which should give us output like the following:
```
        Scope ExecutionPolicy
        ----- ---------------
MachinePolicy       Undefined
   UserPolicy       Undefined
      Process       Undefined
  CurrentUser    RemoteSigned
 LocalMachine       AllSigned
```

Here is more information on the different types of execution policy:

```
-ExecutionPolicy
Specifies the execution policy. If there are no Group Policies and each scope's execution policy is set to Undefined, then Restricted becomes the effective policy for all users.

The acceptable execution policy values are as follows:

* AllSigned. Requires that all scripts and configuration files are signed by a trusted publisher, including scripts written on the local computer.
* Bypass. Nothing is blocked and there are no warnings or prompts.
* Default. Sets the default execution policy. Restricted for Windows clients or RemoteSigned for Windows servers.
* RemoteSigned. Requires that all scripts and configuration files downloaded from the Internet are signed by a trusted publisher. The default execution policy for Windows server computers.
* Restricted. Doesn't load configuration files or run scripts. The default execution policy for Windows client computers.
* Undefined. No execution policy is set for the scope. Removes an assigned execution policy from a scope that is not set by a Group Policy. If the execution policy in all scopes is Undefined, the effective execution policy is Restricted.
* Unrestricted. Beginning in PowerShell 6.0, this is the default execution policy for non-Windows computers and can't be changed. Loads all configuration files and runs all scripts. If you run an unsigned script that was downloaded from the internet, you're prompted for permission before it runs.
```