---
order: 97
icon: device-desktop
---

# The GUI

Path planning can be difficult when using coordinates without any sort of visualization. The Joos GUI aims to make path planning as smooth as possible, so all you have to worry about is where you want the robot to go, not how to make it get there.

![Opening the GUI](/assets/gui-preview.png)

To get started with the GUI, you can either import it into an Android Studio / IntelliJ IDEA project or download it as a standalone application.

### Downloading the GUI

The GUI downloads are available [here](https://github.com/amarcolini/joos/releases). There are platform-specific distributions, so make sure to download the right one for your operating system. After downloading and extracting the zip file, run the `launcher` executable located in the `/bin` folder.

:::filetree 
- /joos-gui-(version)-(platform)
    - /bin
        - ...
        - `launcher` (this one)
        - ...
    - /conf
    - /legal
    - /lib
    - release
:::

### Importing the GUI

To import the GUI, create an empty java gradle module and add the following to your build.gradle file:

```groovy # build.gradle
plugins {
    id 'java-library'
}

repositories {
    //Add jitpack to your repositories block
    maven { url 'https://jitpack.io' }
}

//Don't forget to set java version to 11
java {
    sourceCompatibility = JavaVersion.VERSION_11
    targetCompatibility = JavaVersion.VERSION_11
}

dependencies {
    //Gradle will automatically retrieve the correct dependencies based on your operating system
    implementation "com.github.amarcolini.joos:gui:0.4.7"
}
```


## Usage

The GUI is split into two parts: the editor and the field view. The editor is where you can modify the trajectory as well as the robot size and constraints, and the field view displays the trajectory and allows the waypoints to be dragged (yes, DRAGGED) to fine tune the trajectory.

To add a segment, right click on the waypoint you want to add a segment to, and select the type of waypoint. In the field view the waypoint will appear on top of the waypoint before it, and can be dragged. To export the trajectory, right click on the waypoint list (but not on the waypoints) and select your export method. To start trajectory playback, click on the field view or press space. The trajectory progress bar is also draggable to scrub to specific parts of your trajectory.

Here's a simple demo of the GUI's capabilities:

[!embed](/assets/gui_demo.mp4)
