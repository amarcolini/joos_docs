---
order: 99
icon: download
---

# Installation

## Manual

1. If you haven't already, download the [FTC SDK](https://github.com/FIRST-Tech-Challenge/FtcRobotController) from its GitHub repository.

2. In the `build.dependencies.gradle` file, add the following lines to the repositories block and the Joos import in the dependencies block:

```groovy #5-6,25 build.dependencies.gradle
repositories {
    mavenCentral()
    google() // Needed for androidx
    jcenter()  // Needed for tensorflow-lite
    maven { url = 'https://jitpack.io' }
    maven { url = 'https://maven.brott.dev/' }
    flatDir {
        dirs rootProject.file('libs')
    }
}

dependencies {
    implementation 'org.firstinspires.ftc:Inspection:7.1.0'
    implementation 'org.firstinspires.ftc:Blocks:7.1.0'
    implementation 'org.firstinspires.ftc:Tfod:7.1.0'
    implementation 'org.firstinspires.ftc:RobotCore:7.1.0'
    implementation 'org.firstinspires.ftc:RobotServer:7.1.0'
    implementation 'org.firstinspires.ftc:OnBotJava:7.1.0'
    implementation 'org.firstinspires.ftc:Hardware:7.1.0'
    implementation 'org.firstinspires.ftc:FtcCommon:7.1.0'
    implementation 'org.tensorflow:tensorflow-lite-task-vision:0.2.0'
    implementation 'androidx.appcompat:appcompat:1.2.0'
    implementation 'org.firstinspires.ftc:gameAssets-FreightFrenzy:1.0.0'

    implementation 'com.github.amarcolini.joos:command:0.4.7-alpha'
}
```
If you don't want to utilize all of Joos' capabilities, but just want its core navigation features, you can replace 'command'
with 'navigation' in the import statement.

3. Since Joos uses newer Java features, it isn't compatible with the Android SDK versions 23 and lower. In the `build.common.gradle` file, change `minSdkVersion` from 23 to 24:

```groovy !#6 build.common.gradle
android {
    ...
    defaultConfig {
        signingConfig signingConfigs.debug
        applicationId 'com.qualcomm.ftcrobotcontroller'
        minSdkVersion 24
        //noinspection ExpiredTargetSdkVersion
        targetSdkVersion 28

        ...
    }
    ...
}
...
```

Note that if you are just using the navigation module, this step isn't necessary.

4. If you want to use Joos' FTC Dashboard integration, then you'll need to install Joos' custom annotation processor. Open
the TeamCode `build.gradle` file:

:::filetree 
- /FtcRobotController  
    - /.github  
    - /FtcRobotController   
    - /TeamCode
        - /lib  
        - /src
        - [!badge variant="dark" text="build.gradle"] (this one)
    - /doc  
    - /gradle/wrapper  
    - /libs  
    - /.gitignore  
    - README.md  
    - build.common.gradle  
    - build.dependencies.gradle
    - build.gradle  
    - gradle.properties  
    - gradlew  
    - gradlew.bat  
    - settings.gradle    
:::

And add the annotation processor to the dependencies block:

```groovy !#10
// Custom definitions may go here

// Include common definitions from above.
apply from: '../build.common.gradle'
apply from: '../build.dependencies.gradle'

dependencies {
    implementation project(':FtcRobotController')
    annotationProcessor files('lib/OpModeAnnotationProcessor.jar')
    annotationProcessor "com.github.amarcolini.joos.command:annotation:0.4.7-alpha"
}
```

If you're using Kotlin instead of Java, you'll have to use [KSP](https://kotlinlang.org/docs/ksp-overview.html) to install the annotation processor.

5. That's it! Get started, or take a look at the [reference].

## Quickstart

You can download the quickstart from [this GitHub repository](https://github.com/amarcolini/joos_quickstart) by cloning it into a fresh Android Studio project:

![Downloading the Quickstart](assets/quickstart_download.gif)

After that, you should be good to go! The quickstart comes with additional tuning OpModes, and for further instructions on those, head to the [quickstart reference].