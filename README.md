# PowerShell Automation
+ Windows 11 version 22H2
+ Visual Studio Code latest version
+ PowerShell Extension version 2024.0.0
+ Git
+ Git for Windows
+ GitHub: https://github.com/gtechsltn/PowerShellAutomation
+ [Node.js v20.x with NPM v10.2.23 and NVM v1.1.12](https://betterprogramming.pub/how-to-use-nvm-to-manage-node-js-20-and-npm-9-5effff2deba9)
+ [Ember LTS](https://emberjs.com/releases/lts/)
    + Ember App: http://localhost:5858
    + [Ember Node LTS Support](https://blog.emberjs.com/ember-node-lts-support/)
    + Ember-CLI v5.5.0 and v4.12.1
        + [ember-cli v5.5.0](https://www.npmjs.com/package/ember-cli/v/5.5.0)
        + [ember-cli v4.12.1](https://www.npmjs.com/package/ember-cli/v/4.12.1)
        + [ember-cli-app-version](https://www.npmjs.com/package/ember-cli-app-version/v/6.0.1)
        + [ember-cli-deploy](https://www.npmjs.com/package/ember-cli-deploy/v/2.0.0)
        + [ember-cli-sass](https://www.npmjs.com/package/ember-cli-sass/v/11.0.1)        
        + [ember-cli-update](https://www.npmjs.com/package/ember-cli-update/v/2.0.1)        
        + [ember-uploader](https://www.npmjs.com/package/ember-uploader/v/2.0.0)
        + [ember-useragent](https://www.npmjs.com/package/ember-useragent/v/0.12.0)
+ [Deploy to IIS](https://cli.emberjs.com/release/basic-use/deploying/)
    + [ember-cli-update](https://www.npmjs.com/package/ember-cli-update/v/2.0.1)
    + [Deploying to Multiple Server Environments in Ember](https://dev.to/mattbeiswenger/deploying-to-multiple-server-environments-in-ember-4poc)

# Getting started
+ [Quick Start v2.14.0](https://guides.emberjs.com/v2.14.0/getting-started/quick-start/)
+ [Quick Start v5.4.0](https://guides.emberjs.com/v5.4.0/getting-started/quick-start/)
+ [Quick Start Latest version](https://guides.emberjs.com/release/getting-started/quick-start/)
+ [CMD to PS1](https://www.meziantou.net/convert-cmd-script-to-powershell.htm)
+ [Deploying to Multiple Server Environments in Ember](https://dev.to/mattbeiswenger/deploying-to-multiple-server-environments-in-ember-4poc)

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

### Advanced

**Ember CLI Deploy**

+ https://www.npmjs.com/package/ember-cli-deploy/v/2.0.0
+ https://dev.to/mattbeiswenger/deploying-to-multiple-server-environments-in-ember-4poc

```
ember deploy development
ember deploy test
ember deploy staging
ember deploy production
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

**With /y**

y: suppress prompting to confirm whether you want to overwrite a file

```
xcopy /s /y "D:\PowerShellAutomation\PowerShellAutomation\ThirdSightPortal\dist\" "C:\inetpub\wwwroot\HTML_CSS_JavaScript_Jquery_Bootstrap_Ember\"
xcopy /s /y "D:\PowerShellAutomation\PowerShellAutomation\ThirdSightPortal\dist\*.*" "C:\inetpub\wwwroot\HTML_CSS_JavaScript_Jquery_Bootstrap_Ember\"
```

**Without /y**

```
xcopy /s "D:\PowerShellAutomation\PowerShellAutomation\ThirdSightPortal\dist\*.*" "C:\inetpub\wwwroot\HTML_CSS_JavaScript_Jquery_Bootstrap_Ember\"
```
