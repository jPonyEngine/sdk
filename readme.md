# jMonkeyEngine SDK [![Build Status](https://travis-ci.org/jMonkeyEngine/sdk.svg?branch=master)](https://travis-ci.org/jMonkeyEngine/sdk)

This repo holds the Legacy SDK of jMonkeyEngine, based on Netbeans platform.

## Getting started
You'll need several things to have the SDK build and run:
- java, if you don't know what it is or how to install it... you'd better run from here.
- [gradle](http://gradle.org/gradle-download/) to build the project. (Included in the repo)
- [NetBeans](https://netbeans.org/downloads/) to edit and run the project (First build will download it to ```netbeans/```).
- Then you need to read this [documentation](http://wiki.jmonkeyengine.org/doku.php/sdk:development) thoroughly.

Before hopping into Netbeans plugin development you'll need to build the project using gradle command:
```
gradlew(.bat) buildSdk
```

Then you'll be able to open the project in the bundled netbeans and start your devs.

## Creating a distribution of the SDK
When in Netbeans right click on the SDK project and choose : **package as...**
then choose whatever distribution you fancy.
![Package as...](http://i.imgur.com/5V2uBHf.png)

If you however want to Debug an SDK Issue you click on Run/Debug instead
## jMonkeyEngine3 version
The sdk uses jME published artifacts.
You can change the version of these artifacts by editing the build.gradle file and changing the ext.jmeFullVersion variable to a proper version.

### Using jME official release
jME official release are published on [jcenter](https://bintray.com/bintray/jcenter). You'll find there all the releases listed [here](https://github.com/jMonkeyEngine/jmonkeyengine/releases) since jME 3.1.0-alpha2
To use jcenter as a repository just put :
```
jcenter()
```
or
```
jcenter {
    url "http://jcenter.bintray.com/"
}
```
in the repositories section of the build.gradle.
Then you have to change the jmeFullVersion to whatever official release version.

<i>example : 3.1.0-alpha2</i>

### Using jME SNAPSHOT versions
jME is built on each commit, and a SNAPSHOT version is done and published on a [custom public repo](http://updates.jmonkeyengine.org/maven/)
To use this repository just put :
```
maven {
    url "http://updates.jmonkeyengine.org/maven/"
}
```
in the repositories section of the build.gradle.
Then you have to change the jmeFullVersion to a SNAPSHOT version.

<i>example : 3.1.0-SNAPSHOT</i>
> WARNING !!! Note that depending on how often gradle updates your dependencies, using SNAPSHOT version can break your build any time

### Using jME from any git branch or commit
You can use [jitpack](https://jitpack.io/) as a repository to be able to build jME dependencies from any branch or commit.
To use jitpack as a repository just put :

```
maven {
    url "https://jitpack.io"
}
```
in the repositories section of the build.gradle.
Then you have to change the jmeFullVersion to a branch or commit tag (see [jitpack documentation](https://jitpack.io/docs/)).

<i>example : PBRisComing-SNAPSHOT</i>
> WARNING !!! Note that depending on how often gradle updates your dependencies, using branch or commit dependency version can break your build any time
