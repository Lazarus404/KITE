# This is KITE 2.0, Karoshi Interoperability Testing Engine (version 2.0)

The effortless way to test WebRTC compliance, prevent [Karoshi](https://en.wikipedia.org/wiki/Kar%C5%8Dshi) with __KITE!__


#### _This is not an official Google product_

See [LICENSE](LICENSE) for licensing.
&nbsp;    

## A. Install prerequisite software  

You will need Git, JDK 8 and Maven. Here's where you can find them:

* [Git](https://git-scm.com/downloads)  
* [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)  
* [Maven](https://maven.apache.org/download.cgi?Preferred=ftp://mirror.reverse.net/pub/apache/)  

To verify your setup, in a command prompt or shell window, type
``` 
mvn -version
```
Expected output on Windows 10:
```
Apache Maven 3.6.1
Maven home: C:\Program Files\Maven\apache-maven-3.6.1\bin\..
Java version: 1.8.0_191, vendor: Oracle Corporation
Java home: C:\Program Files\Java\jdk1.8.0_191\jre
Default locale: en_US, platform encoding: Cp1252
OS name: "windows 10", version: "10.0", arch: "amd64", family: "windows"
```

Install your favorite Java IDE. We recommend [IntelliJ IDEA Community](https://www.jetbrains.com/idea/download) but you can use Eclipe or any other IDE if you prefer.
&nbsp;    

## B. Install KITE 2.0

1. Clone this repo, for example under `\GitHub\`:  
    ```
    cd \
    mkdir GitHub
    cd GitHub
    git clone https://github.com/webrtc/KITE.git
    cd KITE
    ```

2. Configure __KITE__  

    This will set KITE_HOME environment variable and add utility scripts to your path.  

    2.1 On Windows, open a Command Prompt window and:
    ```
    cd \GitHub\KITE
    configure.bat
    ```

    2.2 On linux or mac, open a Command Prompt window and:
    ```
    cd \GitHub\KITE
    chmod +x configure
    configure
    ```

3. Compile 

    Just type `c` (which will execute `mvn clean install -DskipTests`)
    ```
    cd \GitHub\KITE
    c
    ```
    If you are within a test folder, for example in KITE-AppRTC-Test, you can type __`c`__ to compile the test module
     only or __`c all`__ to recompile the entire project:
    ```
    cd \GitHub\KITE\KITE-AppRTC-Test
    c all
    ```
     
    &nbsp;    

## C. Install the local grid

To setup the local grid, refer to the [local grid setup guide](scripts/README.md).
  
&nbsp;    
&nbsp;      
      

    
## D. Run the sample tests


__Note:__ You will need to have your [local grid](scripts/README.md) running before you can execute any test.  
You can check if your local grid is running and the browser versions installed by 
opening the [Grid Console](http://localhost:4444/grid/console).
In the following example, we are assuming __Chrome__ version __73__ and __Firefox__ version __66__.


### Edit the test config file

Edit the file `./KITE-Example-Test/configs/example.config.json` with your favorite text editor.  
You will need to change __`version`__ and __`platform`__ according to what is installed on your local grid.
For example, if your local grid is windows and the latest stable version of __Chrome__ is __73__, you should set: 
```json
      "version": "73",
      "platform": "WINDOWS",
```

If you're using Linux or Mac, change "WINDOWS" to "LINUX" or "MAC".



Read below about the configuration file, check that the desired browsers listed in your configuration file are available in your system.

### Understanding a basic configuration file

The example example.config.json file is almost the simplest config file you can get
 (Change the version of browsers to the appropriated one that you have installed on your testing machine):

```json
{
  "name": "Kite test example (with Allure reporting)",
  "callback": null,
  "remotes": [
    {
      "type": "local",
      "remoteAddress": "http://localhost:4444/wd/hub"
    }
  ],
  "tests": [
    {
      "name": "KiteExampleTest",
      "tupleSize": 1,
      "description": "This example test opens google and searches for Cosmo Software Consulting and verify the first result",
      "testImpl": "KiteExampleTest",
      "payload" : {
        "test1": "ONE",
        "test2": "TWO"
      }
    }
  ],
  "browsers": [
    {
      "browserName": "chrome",
      "version": "73",
      "platform": "WINDOWS",
      "flags": []
    },
    {
      "browserName": "firefox",
      "version": "66",
      "platform": "WINDOWS",
      "flags": []
    }
  ]
}

```

It registers only selenium server in the local machine:
```json
  "remotes": [
    {
      "type": "local",
      "remoteAddress": "http://localhost:4444/wd/hub"
    }
  ],
```

It registers IceConnectionTest class as a test (this class is implemented in KITE-AppRTC-Test)
```json
  "tests": [
    {
      "name": "KiteExampleTest",
      "tupleSize": 1,
      "description": "This example test opens google and searches for Cosmo Software Consulting and verify the first result",
      "testImpl": "KiteExampleTest",
      "payload" : {
        "test1": "ONE",
        "test2": "TWO"
      }
    }
  ],
```

It requests for firefox and chrome. Version and platform are required fields. Version and platform actually used in the tests will be reported in the result, and will appear in the dashboard.

Sample config files in `KITE-Example-Test/configs` contain the example with different browser, version and platform configuration, take a closer look

```json
  "browsers": [
    {
      "browserName": "chrome",
      "version": "72",
      "platform": "LINUX",
      "flags": []
    },
    {
      "browserName": "firefox",
      "version": "65",
      "platform": "MAC",
      "flags": []
    }
  ]
```


#### Run KITE-Example-Test


To run the AppRTC iceconnection test:
```
cd %KITE_HOME%\KITE-Example-Test
r example.config.json
```

#### Run KITE-AppRTC-Test

Edit the file `./KITE-AppRTC-Test/configs/iceconnection.local.config.json` with your favorite text editor.  
You will need to change __`version`__ and __`platform`__ according to what is installed on your local grid.

To run the AppRTC iceconnection test:
```
cd %KITE_HOME%\KITE-AppRTC-Test
r iceconnection.local.json
```

Alternatively, you can launch the test with the full command.
On Windows:  
```
-Dkite.firefox.profile="%KITE_HOME%"/third_party/ -cp "%KITE_HOME%/KITE-Engine/target/kite-jar-with-dependencies.jar;target/*" org.webrtc.kite.Engine configs/iceconnection.local.json
```
On Linux/Mac:  
```
-Dkite.firefox.profile="$KITE_HOME"/third_party/ -cp "$KITE_HOME/KITE-Engine/target/kite-jar-with-dependencies.jar:target/*" org.webrtc.kite.Engine configs/iceconnection.local.json
```

### Open the dashboard

After running the test, you can open the Allure dashboard with the command `a`.
```
cd %KITE_HOME%\KITE-AppRTC-Test
a
```


Congratulation! You should see the results of your first KITE test.

![KITE Test Dashboard](third_party/allure-2.10.0/lib/Alluredashboard.png)  




Alternatively, the full command to launch the Allure dashboard is:  
```
allure serve PATH_TO/kite-allure-reports
```



