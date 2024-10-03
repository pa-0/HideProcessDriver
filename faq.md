# Frequently Asked Questions

1. ### How do I find my system's Unique Process ID? 

    To get the _Unique Process ID_, follow the steps below:
    - Attach `WinDbg - Kernel Debugger` to your machine
    - Issue the command:
      ```console
      dt nt!_EPROCESS  
      ```
    - This will generate a response. There you can search for `UniqueProcessId` field and get the corresponding `OFFSET` for your version


2. ###  Do I only need to set the correct OFFSET once per OS version or once per MACHINE?

    It may hold on different Minor Versions. But mostly they change on the Major Versions. 
    
    For example, if you want to make it work in Windows 10 after setting it in Windows 11 you'll need to update the offsets. It wont work on all previous versions


3. ### If i want it to be compatible with all Windows versions (7 to 11), I have to determine and set OFFSET using `WinDbg` for each version one-by-one? Isn't there a faster way to do that? How do I include OFFSET's for each version in the code?

    It won't be easy. You can scan through memory and look for what you need or parse symbols. Its a pretty difficult challenge, especially the latter... 
    
    Alternatively, a more doable approach is to check the version and then use conditions to determine what offset to use. You can also find your own creative way, just keep trying.  


4. ### How do I edit in the driver code to hide a process by NAME instead of `PID`, e.g., hiding a process by searching for '`appname.exe`'

    Invest some time with the structure `_EPROCESS` I already told you about. It contains many fields one of them holds the files name. From that field you can just compare it to your targets name. But you'll need to change the communication part, from sending the PIDs number to a string. 
    
    Otherwise, do it from the user-mode part. It will not be difficult to find examples of getting the `PID` from a process' name then you don't need to change as much of the code...  
    
    Experiment around. I recommend first trying the second approach and then when you have it working and understand it, try to achieve the same from kernel, its a challenge but definitely doable
