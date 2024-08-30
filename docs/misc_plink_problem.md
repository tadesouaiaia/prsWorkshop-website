

# MacOs Permission Errors 

![Screenshot](images/mac_plink.png)

If you see this error, **Do not click: Move To Trash**. 

Instead click on the top right hand corner of the box, or stop running whatever program you are using and follow the
 instructions below to give your system permission to run downloaded software. 
to give your system permission to run downloaded software. 


!!! warning "Shortcut: Users report that this shortcut can also be used to change program permissions:" 
 	1. User finder navigate to the file in question. 
	2. Right click on the icon and select **open**. 
	3. A warning will pop-up.  Click open and this program can be run in the future. 
	![Screenshot](images/mac_ex.png)










## 1- Change Default Settings
By default macOS allows you to open apps from the official Mac App Store only, If you have this set as your default 
you can change this if you: 
    
1. Open System Preferences.
2. Go to the Security & Privacy tab.
3. Click on the lock and enter your password.
4. Change settings to include identified developers (see below): 



![Screenshot](images/mac_plink2.png)



## 2- Allow Exceptions 


Expanding permissions to include "identified developers" is required but not sufficient.  To obtain 
further permission to run **Plink** or any other unapproved program you can:  


1. Open System Preferences.
2. Go to Security & Privacy and select the General tab.
3. If this has happened within the hour, this page will give you an override button to open **Open Anyway**. 
4. Enter you password as above and click this button.
5. You will be asked to once more which will create an exception allowing you to run Plink in the future. 

![Screenshot](images/mac_plink3.jpg)


## BridgePRS specific errors 

If this problem has occured when running bridgePRS and you have moved plink to the trash you will have to recover it. 
Additionally the empty files created by a failed attempt to run Plink can cause problems if BridgePRS tries to recover your progress. 


!!! help "Restarting a bridgePRS run"
    You can avoid this problem by manually deleting your output directory and starting over, or by using the restart flag: 
        ```
        $./bridgePRS pipeline go -o out1 --config_files data/afr.config data/eur.config --phenotype y --restart
        ```
    This will force bridgePRS to restart every subprogram from the beginning.     







 
