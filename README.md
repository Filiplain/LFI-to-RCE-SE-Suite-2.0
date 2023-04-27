# Remote Code Execution in SoftExpert Excellence Suite 2.0 - CVE-2023-30330
Authenticated Local File Inclusion to Remote Code Execution on SoftExpert Excellence Suite EQM.

[CVE-2023-30330](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2023-30330)

* SE Suite 2.x versions before 2.1.3 
* Tested on versions: 2.0.15.31 and 2.0.15.115

## LFI PoC
https://github.com/Filiplain/LFI-to-RCE-SE-Suite-2.0/tree/main/PoC

![](https://github.com/Filiplain/LFI-to-RCE-SE-Suite-2.0/blob/main/Images/poc-script.png?raw=true)

## 1- Local File Inclusion:

The researcher was able to find a PHP function **"/se/v42300/generic/gn_defaultframe/2.0/defaultframe_filter.php"** that includes a file with extension **".inc"** in a
base64 encoding format through the **“managerPath”** parameter in a POST request. By changing the **“.inc”** file to for example **“C:\windows\win.ini”** and converting it back to base64, we are going to get that file on the system.

![](https://github.com/Filiplain/RCE-SE-Suite-2.0/blob/main/Images/LFI-0.png?raw=true)
![](https://github.com/Filiplain/RCE-SE-Suite-2.0/blob/main/Images/LFI-1.png?raw=true)

The PHP file **"defaultframe_filter.php"** is using the function “require_once()” to include the files, this function could allow a hacker to execute Remote Code Execution by including poisoned logs.

![](https://github.com/Filiplain/RCE-SE-Suite-2.0/blob/main/Images/require-once.png?raw=true)

## 2- Remote Code Execution:

* We can include and read PHP error logs.
* We can use require_once() to execute PHP code.
* We need to insert PHP code into the error logs.

By analyzing the **“user_action.php”** function when uploading a new profile picture, we can notice
that when you upload an invalid or malicious image, the page will throw an error 401. After causing
the error you can read the logs again and see that some parameters like the “Referer” header will be
logged, knowing this we can inject PHP code into the **“Referer”** header.

![](https://github.com/Filiplain/RCE-SE-Suite-2.0/blob/main/Images/Inject-PHP.png?raw=true)

After trying to upload the malicious image and injecting PHP code into the Referer, we can confirm
the log poisoning by reading the logs.

![](https://github.com/Filiplain/RCE-SE-Suite-2.0/blob/main/Images/Logged-rce.png?raw=true)

Now we can execute commands by reading the logs.
**Command:** whoami

![](https://github.com/Filiplain/RCE-SE-Suite-2.0/blob/main/Images/whoami.png?raw=true)



