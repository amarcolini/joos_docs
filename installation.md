---
order: 99
icon: download
---

# Installation

## Manual

1. If you haven't already, download the [FTC SDK](https://github.com/FIRST-Tech-Challenge/FtcRobotController) from its GitHub repository.

2. Open the TeamCode `build.gradle` file:

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

3. In the `build.gradle` file in the TeamCode folder, add the following repositories block and the Joos imports in the dependencies block:

```groovy #2-5,23-24 build.gradle
// Custom definitions may go here
repositories {
    maven { url = 'https://jitpack.io' }
    maven { url = 'https://maven.brott.dev/' }
}

// Include common definitions from above.
apply from: '../build.common.gradle'
apply from: '../build.dependencies.gradle'

android {
    namespace = 'org.firstinspires.ftc.teamcode'

    packagingOptions {
        jniLibs.useLegacyPackaging true
    }
}

dependencies {
    implementation project(':FtcRobotController')
    annotationProcessor files('lib/OpModeAnnotationProcessor.jar')

    implementation "com.amarcolini.joos:command:0.4.9"
    annotationProcessor "com.amarcolini.joos:annotation:0.4.9"
}
```

- If you don't want to utilize all of Joos' capabilities, but just want its core navigation features, you can replace 'command' with 'navigation' in the import statement and remove the annotation processor.

- If you're using Kotlin instead of Java, you'll have to use [KSP](https://kotlinlang.org/docs/ksp-overview.html) to install the annotation processor.

4. That's it! Get started, or keep reading.

## Quickstart

You can download the quickstart from [this GitHub repository](https://github.com/amarcolini/joos_quickstart) by cloning it into a fresh Android Studio project:

![Downloading the Quickstart](assets/quickstart_download.gif)

After that, you should be good to go! The quickstart comes with additional tuning OpModes, and for further instructions on those, head to the quickstart.

[!ref](/quickstart/quickstart_overview.md)