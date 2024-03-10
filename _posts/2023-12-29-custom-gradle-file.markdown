---
layout: post
title: Create custom gradle files to optimize your project
date: 2023-12-27 12:00:00 +0300
description: How to create custom gradle files to optimize your project
img: gradle.jpg 
tags: [Kotlin, Gradle, Optimize]
---

When configuring large projects with multiple modules, it's common to encounter a significant amount of boilerplate code in Gradle files. This guide aims to address this issue by presenting a method to eliminate unnecessary redundancy and retain only essential information in each Gradle file.

### Identifying the Problem

The primary challenge we tackle in this guide pertains to boilerplate code when dealing with multiple modules. While most modules share a common configuration, there are specific features that vary. In the example below, you'll see a Gradle file for a module, and the 'Android' block is identical across all modules in the project.

```kotlin
plugins {
    id "com.android.library"
    id "kotlin-android"
    id "androidx.navigation.safeargs.kotlin"
    id "kotlin-parcelize"
    id "kotlin-kapt"
}

android {
    compileSdk Config.compileSdk

    defaultConfig {
        minSdkVersion Config.minSdk
        targetSdkVersion Config.targetSdk

        testInstrumentationRunner Config.testInstrumentationRunner
        consumerProguardFiles Config.proguardFiles
    }

    buildFeatures {
        dataBinding true
        viewBinding true
    }

    buildTypes {
        production {
            minifyEnabled true
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
            debuggable false
        }
        release {
            minifyEnabled true
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
            debuggable false
            signingConfig signingConfigs.debug
        }
        debug {
            minifyEnabled false
            debuggable true
            signingConfig signingConfigs.debug
        }
    }
    compileOptions {
        sourceCompatibility Config.javaVersion
        targetCompatibility Config.javaVersion
    }
    kotlinOptions {
        jvmTarget = Config.jvmTarget
    }

    namespace 'com.example.module1'
}

dependencies {
    implementation project(Modules.BASE)
    implementation project(Modules.UI_COMPONENTS)
    implementation project(Modules.NAVIGATOR)
}
```

### Creating a Custom Configuration File

To address this issue, let's create a custom file to store our Android block configuration. Switch your view to Project to create a new file at the project's root. We'll name this file 'feature.gradle' for now, but feel free to choose any name, ensuring it ends with '.gradle'.

![Creating custom file]({{site.baseurl}}/assets/img/posts/post3/img1.png)

Now, let's add the Android block to our 'feature.gradle' file:

```kotlin
android {
    compileSdkVersion Config.compileSdk

    defaultConfig {
        minSdkVersion Config.minSdk
        targetSdkVersion Config.targetSdk

        testInstrumentationRunner Config.testInstrumentationRunner
        consumerProguardFiles Config.proguardFiles
    }

    buildFeatures {
        dataBinding true
        viewBinding true
    }

    buildTypes {
        production {
            minifyEnabled true
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
            debuggable false
        }
        release {
            minifyEnabled true
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
            debuggable false
            signingConfig signingConfigs.debug
        }
        debug {
            minifyEnabled false
            debuggable true
            signingConfig signingConfigs.debug
        }
    }
    compileOptions {
        sourceCompatibility Config.javaVersion
        targetCompatibility Config.javaVersion
    }
    kotlinOptions {
        jvmTarget = Config.jvmTarget
    }
}
```

### Applying the Configuration File to Modules

To integrate this file into our modules, we'll reference it using the 'apply from' command:

```kotlin
plugins {
    id "com.android.library"
    id "kotlin-android"
    id "androidx.navigation.safeargs.kotlin"
    id "kotlin-parcelize"
    id "kotlin-kapt"
}

apply from: '..\\features.gradle'

android {
    namespace 'com.example.module1'
}

dependencies {
    implementation project(Modules.BASE)
    implementation project(Modules.UI_COMPONENTS)
    implementation project(Modules.NAVIGATOR)
}
```

### Conclusion

Implementing this approach significantly streamlines Gradle files, eliminating repetitive code. This method is versatile and can be extended to other blocks. For instance, I created a 'sensitive.gradle' file to store user and password information for a company, utilizing a project.ext variable for their personal repository. This way, developers only need to modify this file, simplifying the configuration process and minimizing disruptions to the Gradle setup.

Integrating this strategy with [buildSrc]({% link _posts/2023-12-17-buildsrc-guide.markdown %}) further optimizes your code.

