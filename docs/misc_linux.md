

# Linux for Windows Users

For windows users, a single **WSL** command can be used to install linux.  Detailed information on WSL can be found on the [microsoft website](https://learn.microsoft.com/en-us/windows/wsl/install).

!!! tips "Using Administrator Mode to Installing Linux"                                                                                                                                                                                     
    Open PowerShell or Windows Command Prompt by right-clicking and selecting "Run as administrator", and type: 
    ```                                                                                                  
    wsl --install                                                                                                                    
                                                                                                                                                                                                                            
    ```                                                                                                                                                                                                                     
    This command will enable the features necessary to run WSL, restart your computer, and install the Ubuntu distribution of Linux.  

    ![Screenshot](images/linux_install.png) 

    Once it has been installed you will need to create a new user account, to learn more about this please see [microsoft best practices](https://learn.microsoft.com/en-us/windows/wsl/setup/environment#set-up-your-linux-username-and-password)
    Once finished you can run linux using [windows terminal](https://learn.microsoft.com/en-us/windows/terminal/install) or by going to the start menu and typing "Ubuntu".  This will open Linux in its own console
    window.
