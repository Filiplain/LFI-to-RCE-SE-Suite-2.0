## PoC for Local File Inclusion in SE Suite 2.x before 2.1.3 - CVE-2023-30330

Author: Filiplain
Local File Inclusion in: SE Suite 2.x before 2.1.3
Tested on versions: 2.0.15.31 and 2.0.15.115

### Usage
* Make sure that user is logged out before running PoC
* Edit PoC if needed (Example: page target could be http not https)

Example: 
~~~
./lfi-poc.sh fakepage.com admin P@sword123 "C:/windows/win.ini" 
~~~

![](https://github.com/Filiplain/LFI-to-RCE-SE-Suite-2.0/blob/main/Images/poc-script.png?raw=true)
