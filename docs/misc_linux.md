

# Linux for Windows Users

For windows users, a single **WSL** command can be used to install linux.  Detailed information on WSL can be found on the [microsoft website](https://learn.microsoft.com/en-us/windows/wsl/install) and 
step-by-step video obstructions can be found [here](https://www.youtube.com/watch?v=cdIQ61eCaqE).  These steps involve: 

## Installation 

!!! tips "Using Administrator Mode to Installing Linux"                                                                                                                                                                                     
    Open PowerShell or Windows Command Prompt by right-clicking and selecting "Run as administrator", and type: 
    ```
    wsl --install -d Ubuntu    
    ```


    This command will enable the features necessary to run WSL, restart your computer, and install the Ubuntu distribution of Linux.  

    ![Screenshot](images/linux_install.png) 

    After it has been installed you will need to create a new username and password, to learn more about this please see [microsoft best practices](https://learn.microsoft.com/en-us/windows/wsl/setup/environment#set-up-your-linux-username-and-password)
    

## Setup 

Once Linux has been installed and you have created a new account you can run Ubuntu using [windows terminal](https://learn.microsoft.com/en-us/windows/terminal/install) or by going to the start menu and typing "Ubuntu".  Additionally, you should see that **Ubuntu** has been added to your applications and that there is a **Ubuntu Folder** on your computer (alongside Desktop, Downloads, etc).  Double clicking the Ubuntu app will open up the Ubuntu terminal in the **Ubuntu folder**. 

You should see many sub-folders within the Ubuntu folder (such as bin, home, etc, etc.) .  You can view them using [terminal commands](prep_terminal.md) or by double-clicking on them. 
If you go into the **home/** folder you should see another folder with your username.  This folder (**home/(your username)/**) is where you should store your workshop folders and run programs during the workshop. 

Once you have successfully installed Ubuntu, you can follow the rest of the [pre-workshop tutorial](prep_list.md) using the Linux instructions. 


