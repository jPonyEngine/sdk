# Maintaining the SDK  
This document is _only_ for developers who intend to maintain the SDK.  
If you don't fulfill this criteria, feel free to skip that part as it won't help you with anything.  
Actually this is not a real document either but simply a collection of links, hints, notes, etc.  

### How to Release a new Version
Since we have automated builds with travis you only have to tag a commit and it will be released for you.  
Keep in mind that I currently have the impression that you can only publish one release at a time.  
The OAUTH Key for this deployment is found in the .travis.yml along with instructions on how to generate such a file. You only need this when I'm (MeFisto94) no longer permitted to push to the repo.  

The build process with the netbeans installers (`ant build-installers`) is a bit fragile especially in Travis Environment (small diskspace, no root (because caching)) so handle with care.  

### What has to be done for a new Version?
Now this part is actually for me so I don't forget it :P  

Change http://wiki.jmonkeyengine.org/doku.php/sdk:welcome:3_1 every tag to keep up with the version number and then save it as nbres:/com/jme3/gde/docs/sdk/welcome/local.html  

### How to Upgrade Dependencies
See `nbi/stub/ext/infra/build/products/README` for now. It will be included in the docs and so the README will only be a link to that.  

See `resources/README.md` and `harness-override/README.md` for how to change the Netbeans Icon (on Windows)  

### How to add a new template to File->New
This is really easy once we had fixed the issue #33.  
Basically in your Module, add a `package-info.java`. For example:  
```
@TemplateRegistration(folder = "Scene", content = "Scene.j3o", displayName = "#Templates/JME3/Scene.j3o", description = "EmptyScene.html")
package com.jme3.gde.scenecomposer;
import org.netbeans.api.templates.TemplateRegistration;
```

As you can already see here, Folder is where it will appear, content is the package relative path (so you have to place the file inside of the source folder to have it appear in the jar), displayName is the key to look in `Bundle.properties`. Description **HAS TO BE** an external file. Neither keys nor in-line works.  

IF you are using any `.j3XYZ` file, it will work fine, `.blend` also works, however `.jpg` for example wont.  
The thing here is that Netbeans tries to interpret such files as text, replacing Strings such as the name and date (which is useful for source code indeed), but breaks our binary files (since we don't have valid UTF-8).  
If you are interested in the background, see Issue 33 and the related code changes, however the simple answer is:  
In Module `SceneComposer` under `com.jme3.gde.scenecomposer` you'll find the `CopyTemplateHandler.java` file.  
Edit the following line just so it recognizes the appropriate extensions.  
```java
return ext.startsWith("j3") || ext.equals("blend"); /* Add your own binary extensions here !! */
```

### How to update Avian (iOS JVM)
Avian is bundled in the jme3-ios.jar as `nbres:/com/jme3/gde/ios/avian-openjdk-mac.zip`.  
The actual upgrade process is simply downloading the new versions, executing `cd jme3-ios && ant package-avian` and update the avianVersion in `IosCompositeProvider` or rather in it's inner SavePropsListener class.  

- Download [Avian](https://readytalk.github.io/avian/) into any desired directory and extract it (it should create a folder called avian)
- Download the OpenJDK unofficial builds from [here](https://github.com/alexkasko/openjdk-unofficial-builds). It's an unofficial build since there is only the source available for Mac OS and/or the only way to get official builds would be through the linux package managers. You need the "zip" file (no installer).  
- Get the OpenJDK Sources. They are a bit hidden, but essentially pick the version [here](http://hg.openjdk.java.net/jdk7/jdk7/jdk/tags). Then click on zip (or whatever you like) on the left menu and extract the sources.
- Download the Hello iOS Example Project from [here](https://github.com/ReadyTalk/hello-ios) (Click on Download ZIP) and extract it.

Now you have to set-up the paths to the freshly downloaded things.  
Open `jme3-ios/nbproject/project.properties` and adopt the changes to the few given paths. Let it point to the extracted things you just downloaded.
Issue `cd jme3-ios && ant package-avian`.

If you experience `ln: /usr/include/netinet/ip.h: No such file or directory` or something, then you didn't have XCode Command Line Tools installed. Type `xcode-select --install` to install them.  
Note: Personally I was unable to compile Avian from here due to several compilation faults related to the downloaded JDK Sources. I will edit this document once I was able to.  