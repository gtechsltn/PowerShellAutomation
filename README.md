# PowerShell Automation
+ Windows 11 version 22H2
+ Visual Studio Code latest version
+ PowerShell Extension version 2024.0.0
+ Git and Git for Windows
+ GitHub: https://github.com/gtechsltn/PowerShellAutomation
+ Ember.js
+ [Deploy to IIS](https://cli.emberjs.com/release/basic-use/deploying/)
+ Ember App: http://localhost:5858

# Getting started

### Develop

```
ember serve --port 4300
```

**Step-By-Step**

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

echo "Success develop"

echo "Check resoult  folder .\dist\"
```

### Deploy

+ https://dev.to/mattbeiswenger/deploying-to-multiple-server-environments-in-ember-4poc
+ build environments:
    + development
    + test
    + production

**ember build -prod**

```
ember build --environment production  --output-path dist/
```

**Step-By-Step**

```
md "C:\inetpub\wwwroot\HTML_CSS_JavaScript_Jquery_Bootstrap_Ember"

echo "Open Command Prompt as an Administrator"
cd "D:\PowerShellAutomation\PowerShellAutomation\ThirdSightPortal"
xcopy /s D:\PowerShellAutomation\PowerShellAutomation\ThirdSightPortal\dist\*.* C:\inetpub\wwwroot\HTML_CSS_JavaScript_Jquery_Bootstrap_Ember\

echo "Or with PowerShell run as an Administrator"
Copy-Item -Path D:\PowerShellAutomation\PowerShellAutomation\ThirdSightPortal\dist\ -Destination C:\inetpub\wwwroot\HTML_CSS_JavaScript_Jquery_Bootstrap_Ember\ -Force

echo "Open CMD as an Administrator"
cd "D:\PowerShellAutomation\PowerShellAutomation\ThirdSightPortal"

echo "Add AppPool"
%systemroot%\system32\inetsrv\APPCMD add apppool /name:"HTML_CSS_JavaScript_Jquery_Bootstrap_Ember" /managedRuntimeVersion:v4.0

echo "Add Site"
%systemroot%\system32\inetsrv\APPCMD add site /name:"HTML_CSS_JavaScript_Jquery_Bootstrap_Ember" /bindings:http://*:5858 /physicalpath:"C:\inetpub\wwwroot\HTML_CSS_JavaScript_Jquery_Bootstrap_Ember"

echo "Set Site Name, Site Path and Set AppPool"
%systemroot%\system32\inetsrv\APPCMD set site /site.name:"HTML_CSS_JavaScript_Jquery_Bootstrap_Ember" /[path='/'].applicationPool:"HTML_CSS_JavaScript_Jquery_Bootstrap_Ember"

echo "Set AppPool to Classic .NET AppPool"
%systemroot%\system32\inetsrv\AppCmd.exe set app "HTML_CSS_JavaScript_Jquery_Bootstrap_Ember/" /applicationPool:"Classic .NET AppPool"

echo "Set defaultDocument Enable = True"
%systemroot%\system32\inetsrv\AppCmd.exe set config "HTML_CSS_JavaScript_Jquery_Bootstrap_Ember" /section:defaultDocument /enabled:true

echo "Set defaultDocument to 'index.html'"
%systemroot%\system32\inetsrv\AppCmd.exe set config "HTML_CSS_JavaScript_Jquery_Bootstrap_Ember" /section:defaultDocument /+files.[value='index.html;']

echo "Set directoryBrowse Enable = True"
%systemroot%\system32\inetsrv\AppCmd.exe set config "HTML_CSS_JavaScript_Jquery_Bootstrap_Ember" /section:directoryBrowse /enabled:true

echo "Edit file web.config"
notepad C:\inetpub\wwwroot\HTML_CSS_JavaScript_Jquery_Bootstrap_Ember\web.config

echo "Don't forget to include your ssl keys in your config"

echo "Success deploy"

echo "Start ember application"
start http://localhost:5858
```

### File: web.config

Important note that remove ';' character from **value = "index.html;"** to **value = "index.html"**

```
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <system.webServer>
        <defaultDocument enabled="true">
            <files>
                <clear />
                <add value="index.html" />
            </files>
        </defaultDocument>
        <directoryBrowse enabled="true" />
    </system.webServer>
</configuration>
```

### XCopy
+ https://stackoverflow.com/questions/7170683/copy-all-files-and-folders-from-one-drive-to-another-drive-using-dos-command-pr
```

```
