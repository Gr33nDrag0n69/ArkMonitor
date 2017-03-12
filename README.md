 ![##Images_README_Header##](./PNG/Header.png)

ArkMonitor is a stand-alone PowerShell script to do end-user monitoring on Ark MainNet Node(s) and Delegate. It uses PowerShell so it run only on Windows.

It uses HTTP protocol to communicate with Ark API.

Included features are:

 - Monitoring Last Block Age of every provided nodes URLs.
 - Monitoring Last Block Height of every provided nodes URLs.
 - Monitoring Last Forged Block Height of configured delegate on all provided nodes URLs.
 - Monitoring Delegate Forging Status on all provided URLs.

Current Threshold values are 120 seconds for last block age, 12 blocks lag on public nodes and 21 minutes for last forged block age.

If you like it, please vote for me on Ark MainNet for Delegate. :)

Delegate: gr33ndrag0n / Address: MainNet ??? / TestNet AHmrebSWBwwYQpnbqDZJ1NDoSfZTjCrCkv

##**Installation**

Save a copy of ArkMonitor.ps1

This README use this path C:\Scripts\ArkMonitor.ps1, adjust accordingly if you use something else.

##**Configuration, Manual Usage & Testing**

Open the script in your favorite text editor. Basic Notepad WILL work but not recommended. I recommend notepad++ available for free [HERE](https://notepad-plus-plus.org/).

Scroll to line #94

> Configurable Variables | MANDATORY !!! EDIT THIS SECTION !!!


The configuration is splitted in sub-sections:

 - Monitoring
 - E-mail
 - Account
 - Nodes

### **Configuring Monitoring Section**

This script can test 4 things:

 - All nodes last block age (Age in seconds of the last received block)
 - All nodes last block height (Is it lagging compared to public nodes)
 - Delegate forging status
 - Delegate last forged block age

This section allow disabling a specific test.


### **Configuring E-mail Section**

In this section we will configure the address used to send and received the monitoring automatic e-mails.

Config.          | Description                                                                    | Value Example
------------     | -------------                                                                  | -------------
SenderEmail      | This is the e-mail that will be used as sender by the script.                  | = 'ArkMonitor@mydomain.com'
SenderSmtp       | This is the domain or IP address the script will use as SMTP to send messages. | = 'smtp.myinternetprovider.com'
SendErrorMail    | Enable/Disable the sending of errors messages.                                 | = $True
ErrorEmailList   | E-mail List                                                                    | = @('home@mydomain.com', '1234567890@phoneprovider.com')
 
###_About EmailList_

1 entry example:
> @('email@domain.com')

Multi-entries example:
> @('email@domain.com','5556781212@myphoneprovider.com')

You can use the same address for sender and recipient if you want.

###_About "email2sms"_
Most phone provider have email2sms functionnality. Just check with your provider, you probably already have an addres looking like:
> 9995551212@YourPhoneProvider.com

It allow text e-mail sent to this address to be redirected as sms to your phone.

 
###**Configuring Account Section**

Add your Delegate Name and Address. There is a function later to get the public key. See included example.


###**Configuring Nodes Section**

Add the name and http address of all your nodes. See included examples.


#### Retreive Public Key

`.\ArkMonitor.ps1 -ShowPublicKey`

![##Images_README_Header##](./PNG/ShowPublicKey.png)

Edit the script again to add the public key in the Delegate configuration.

#### Test E-mail Configuration

`.\ArkMonitor.ps1 -SendTestEmail`

![##Images_README_Header##](./PNG/SendTestEmail.png)

#### Run manually the check

`.\ArkMonitor.ps1 -ShowMessage`

![##Images_README_Header##](./PNG/ShowMessage.png)

##**Scheduled Task(s) Creation & Testing**

The script do a check and send e-mail if necessary.

It's not a program, so like you would use crontab in Linux, you need to use Task Scheduler in Windows

Open Task Scheduler and Create a New Task. (Use Full GUI not the Wizard)

General -> Name: ArkMonitor

General -> Execute even if user is not connected

Trigger -> New

			* One time
			* Repeat each "5 minutes" for a duration of "infinity"
			
Action  -> New

			* Command:          C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
			* Arguments:        C:\Scripts\ArkMonitor.ps1
			* WorkingDirectory: C:\Scripts\

##**Troubleshooting & Common Error(s)**

**It doesn’t work.**

Verify PowerShell version installed in your computer. Execute the following command:

> $PSVersionTable

The PSVersion must be at least v4.x.
If not, go [HERE](https://www.microsoft.com/en-us/download/details.aspx?id=40855), select your language, download, install and reboot.

When done re-run the test to confirm your version is now v4.x or upper.

**Script work but communication with server fail.**

Verify the configuration of the server (config,json) to allow your IP address in the whitelist section. Don’t forget to restart your Ark client to update the configuration.

**Script is asking confirmation to execute when running it.**

`Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy Bypass`

**Using "Delegate Last Forged Block Age" You Receive "ERROR: Get-ArkBlockList Result is NULL."**

Are you currently an active delegate ? You must be in the 101 active delegate to forge block.
