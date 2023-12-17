---
layout: post
title: Advanced strategies for dependency management in Kotlin modular projects
date: 2023-12-17 00:00:00 +0300
description: A buildSrc configuration guide
img: kotlin.jpg # Add image post (optional)
tags: [Kotlin, buildSrc, Advanced] # add tag
---

In Kotlin modular projects, efficient dependency management is crucial for smooth and organized development. This article explores a recommended practice: the use of buildSrc. We will discuss the benefits of this approach and provide a practical guide to set up and make the most of this tool.

### Why Use buildSrc?

buildSrc stands out as a powerful tool that centralizes dependency configuration in a single location. By avoiding the dispersion of information across Gradle files in individual modules, it promotes cohesion and facilitates future updates. Additionally, buildSrc is initialized before other modules, making its configuration variables accessible for use throughout the project.

### Getting Started with Configuration

Change the view from "Android" to "Project" in the IDE's "Project" tab.

![Change view from "Android" to "Project"]({{site.baseurl}}/assets/img/posts/post1/img1.png)

Create a folder named buildSrc in the project's root directory.

![Create a folder named buildSrc]({{site.baseurl}}/assets/img/posts/post1/img2.png)

![Create a folder named buildSrc]({{site.baseurl}}/assets/img/posts/post1/img3.png)

Create the build.gradle.kts file inside the buildSrc folder and add the 'KOTLIN-DLS' plugin. The version is automatically selected based on the project's dependencies.

![Create the build.gradle.kts file inside the buildSrc folder]({{site.baseurl}}/assets/img/posts/post1/img4.png)

![Create the build.gradle.kts file inside the buildSrc folder]({{site.baseurl}}/assets/img/posts/post1/img5.png)

![Create the build.gradle.kts file inside the buildSrc folder]({{site.baseurl}}/assets/img/posts/post1/img6.png)

Perform a "SYNC PROJECT" to synchronize the changes made so far.

![Perform a "SYNC PROJECT"]({{site.baseurl}}/assets/img/posts/post1/img7.png)

Create the base path for buildSrc at buildSrc/src/main/java and then create the dependencies files. buildSrc identifies objects such as Config, Lib and Version, making them available to configure the Gradle files of the projects. Let's add our project libraries, versions and configuration to these.

![Create the base path for buildSrc]({{site.baseurl}}/assets/img/posts/post1/img8.png)

### Replacing the configuration, libraries, and version

Now, let's replace the configuration, libraries, and versions. Below, you can see how it was implemented in a test project. Note that this project has only one module, making buildSrc not necessary. However, for study purposes, we are including it in this way.

![Replace the configuration, libraries, and versions]({{site.baseurl}}/assets/img/posts/post1/img9.png)

![Replace the configuration, libraries, and versions]({{site.baseurl}}/assets/img/posts/post1/img10.png)

### Bonus Tip 1: Dependency Desynchronization:

When adding buildSrc to established modular projects, dependency desynchronizations may occur. Mitigate this issue by refreshing the Gradle dependencies using the option available in IDEs, such as IntelliJ IDEA.

![Dependency Desynchronization]({{site.baseurl}}/assets/img/posts/post1/img11.png)
![Dependency Desynchronization]({{site.baseurl}}/assets/img/posts/post1/img12.png)

### Conclusion:

Effective dependency management in Kotlin modular projects is essential for maintaining code integrity and scalability. By adopting buildSrc, developers can ensure a consistent development environment, facilitating long-term project maintenance and avoiding version inconsistencies between modules.