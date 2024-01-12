# PowerShell Automation
+ Windows 11 version 22H2
+ Visual Studio Code latest version
+ PowerShell Extension version 2024.0.0
+ Git and Git for Windows
+ GitHub: https://github.com/gtechsltn/PowerShellAutomation

# Getting started

Develop
```
D:
md D:\PowerShellAutomation
cd D:\PowerShellAutomation
git clone https://github.com/gtechsltn/PowerShellAutomation.git
cd .\PowerShellAutomation\
code .
ember version --verbose
ember new ThirdSightPortal --no-welcome
cd .\ThirdSightPortal\
code .
echo "Add file favicon.ico into folder .\public\assets\"
echo "Change content of index.html"
<link rel="icon" type="image/x-icon" href="{{rootURL}}assets/favicon.ico" />
ember build --environment=production
echo "Check folder .\dist\"
```

Deploy
```
md "C:\inetpub\wwwroot\HTML_CSS_JavaScript_Jquery_Bootstrap_Ember"
echo "Open Windows PowerShell as an Administrator"
cd "D:\PowerShellAutomation\PowerShellAutomation\ThirdSightPortal"
Copy-Item -Path .\dist\ -Destination C:\inetpub\wwwroot\HTML_CSS_JavaScript_Jquery_Bootstrap_Ember\ -Force
echo "Open CMD as an Administrator"
cd "D:\PowerShellAutomation\PowerShellAutomation\ThirdSightPortal"
echo "Add AppPool"
%systemroot%\system32\inetsrv\APPCMD add apppool /name:"HTML_CSS_JavaScript_Jquery_Bootstrap_Ember" /managedRuntimeVersion:v4.0
echo "Add Site"
%systemroot%\system32\inetsrv\APPCMD add site /name:"HTML_CSS_JavaScript_Jquery_Bootstrap_Ember" /bindings:http://*:5858 /physicalpath:"C:\inetpub\wwwroot\HTML_CSS_JavaScript_Jquery_Bootstrap_Ember"
echo "Set Site and Set AppPool"
%systemroot%\system32\inetsrv\APPCMD set site /site.name:"HTML_CSS_JavaScript_Jquery_Bootstrap_Ember" /[path='/'].applicationPool:"HTML_CSS_JavaScript_Jquery_Bootstrap_Ember"
echo "Set AppPool to Classic .NET AppPool"
%systemroot%\system32\inetsrv\AppCmd.exe set app "HTML_CSS_JavaScript_Jquery_Bootstrap_Ember/" /applicationPool:"Classic .NET AppPool"
echo "Set Enable defaultDocument"
%systemroot%\system32\inetsrv\AppCmd.exe set config "HTML_CSS_JavaScript_Jquery_Bootstrap_Ember" /section:defaultDocument /enabled:true
echo "Set Enable directoryBrowse"
%systemroot%\system32\inetsrv\AppCmd.exe set config "HTML_CSS_JavaScript_Jquery_Bootstrap_Ember" /section:directoryBrowse /enabled:true
echo "Set defaultDocument to index.html"
%systemroot%\system32\inetsrv\AppCmd.exe set config "HTML_CSS_JavaScript_Jquery_Bootstrap_Ember" /section:defaultDocument /+files.[value='index.html;']
start http://localhost:5858
```
