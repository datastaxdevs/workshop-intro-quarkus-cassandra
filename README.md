# ![ok](https://github.com/DataStax-Academy/AstraPortia/blob/master/0_materials/ico.jpg?raw=true) Workshop Intro to Quarkus and Cassandra

[![Gitpod ready-to-code](https://img.shields.io/badge/Gitpod-ready--to--code-blue?logo=gitpod)](https://gitpod.io/#https://github.com/DataStax-Academy/workshop-spring-data-cassandra)
[![License Apache2](https://img.shields.io/hexpm/l/plug.svg)](http://www.apache.org/licenses/LICENSE-2.0)
[![Discord](https://img.shields.io/discord/685554030159593522)](https://discord.com/widget?id=685554030159593522&theme=dark)

Today we showcase an application using **Apache Cassandra™** as a backend implemented with **Quarkus**, and experience some developer joy with features that often have been an envy for Java programmers - hot reloading, debugging, containerizing and finally generating native code.

There are many other features of the Quarkus platform that we will not be looking into that are widely considered as unique strengths of the platform including testing.

The application we will be using is based on [Jake's port](https://github.com/tjake/todo-astra-react-serverless/) of the [TodoMVC code](https://github.com/tastejs/todomvc/tree/master/examples/react) originally written by [Pete Hunt](https://github.com/petehunt). The example is modified from [https://github.com/huksley/todo-react-ssr-serverless](https://github.com/huksley/todo-react-ssr-serverless).

![SplashScreen](images/tutorials/splash-quarkus-cassandra.png?raw=true)

a screenshot of this simple app is below.

![AstraTodo](images/tutorials/AstraTodos.png?raw=true)

ℹ️ **Objective(s) of workshop**

Whether you're a seasoned developer or relatively new to programming, you will be spending a lot of time in an Integrated Development Environment and the "inner loop" of development with a lightweight CI/CD cycle.

Once you have a stable environment after repeated iterations in the editor, testing environments, profiler, debugger, etc. you push it to the outer loop which has a more robust CI/CD cycle usually backed by gitops, Jenkins and other CI/CD tools.

The objective of today's workshop is to understand how the Quarkus platform simplifies the "inner loop" of development which results in huge developer productivity gains. You will see in today's workshop you can go from plain old Java to containers with relative ease and yet not make a significant change in devlopment.

Although, the Quarkus platform leverages a reactive architecture, the developer tools are drawn from a list that they are familar with and complements them while making those same tools leaner and more performant. This means that Quarkus does not force developers to use reactive programming styles. Additionally, it helps to build better and more performant applications (which is always necessary) and also enhances the day-to-day life of a developer having to bridge the gap between their existing platforms and taking a plunge into microservices, service mesh and so on. Developers can use their current tools and methodologies and let Quarkus take care of being "reactive", or build new fully-reactive applications that take advantage of Quarkus' reactive core.

ℹ️ **Frequently asked questions**

- _Can I run the workshop on my computer?_
  > There is nothing preventing you from running the workshop on your own machine. If you do so, you will need _java jdk11+_, _Graal VM_, _Maven_, an IDE like _VSCode, IntelliJ, Eclipse, Spring STS_. You will have to adapt commands and paths based on your environment and install the dependencies by yourself. **We won't provide support** to keep on track with schedule.

##  Prerequisites

We strive to make our hands-on workshops prerequisites free -- who has the time to install prerequistes? :-)

However, docker login credentials, some familiarity with [Visual Studio Code](https://code.visualstudio.com) and [Giptod](https://gitpod.io) (although not strictly necessary) might help.

## Materials for the Session

It doesn't matter if you join our workshop live or you prefer to do at your own pace, we have you covered. In this repository, you'll find everything you need for this workshop:

- [Slide deck](./slides.pdf)
- [Discord chat](https://bit.ly/cassandra-workshop)
- [Questions and Answers](https://community.datastax.com/)
- [Workshop code](https://github.com/datastaxdevs/quarkus-astra-intro-demo)

## 0. Table of contents

1. [Create Astra DB Instance](#1-create-astra-db-instance)
2. [Create Astra Token](#2-create-astra-token)
3. [Launch Gitpod](#3-launch-gitpod)
4. [Know your Gitpod](#4-know-your-gitpod)
5. [Setup your Application](#5-setup-your-application)
6. [Run Unit test(s)](#6-run-some-unit-tests)
7. [Quarkus Dev Mode](#7-quarkus-dev-mode)
8. [Debugging](#8-debugging)
9. [Packaging](#9-packaging)
10. [Containerizing](#10-containerizing)
11. [Native Image](#11-native-image)
12. [Homework](#12-homework)


## 1. Create Astra DB Instance

_**`ASTRA DB`** is the simplest way to run Cassandra with zero operations at all - just push the button and get your cluster. No credit card required, $25.00 USD credit every month, roughly 80GB of storage and 20 million read/write operations storage monthly (numbers that have gone up in the last few days) - sufficient to run small production workloads._

✅ Register (if needed) and Sign In to Astra DB [https://astra.datastax.com](https://astra.dev/3-23): You can use your `Github`, `Google` accounts or register with an `email`.

_Make sure to chose a password with minimum 8 characters, containing upper and lowercase letters, at least one number and special character_

✅ Choose "Start Free Now"

Choose the "Start Free Now" plan, then "Get Started" to work in the free tier.

You will have plenty of free initial credit which is renewed each month!.

> If this is not enough for you, congratulations! You are most likely running a mid- to large-sized business! In that case you should switch to a paid plan.

(You can follow this [guide](https://docs.datastax.com/en/astra/docs/creating-your-astra-database.html) to set up your free-tier database with the $25 monthly credit.)

![astra-db-signup](images/tutorials/astra_signup.gif)

To create the database:

- **For the database name** - `workshops`. While Astra DB allows you to fill in these fields with values of your own choosing, please follow our recommendations to ensure the application runs properly.

- **For the keyspace name** - `todolist`. It's really important that you use the name "todolist" for the code to work. In short:

| Parameter     | Value     |
|---------------|-----------|
| Database name | workshops |
| Keyspace name | todolist  |

_You can technically use whatever you want and update the code to reflect the keyspace. This is really to get you on a happy path for the first run._

- **For provider and region**: Choose any provider (either GCP, AWS or Azure). Region is where your database will reside physically (choose one close to you or your users).

> **NOTE:** You may see that only certain GCP regions are available unless you go in and unlock all the other GCP regions and AWS/Azure. If you see that, for the purposes of this workshop, please select one of the available GCP regions.

- **Create the database**. Review all the fields to make sure they are as shown, and click the `Create Database` button.

You will see your new database `pending` in the Dashboard.

![db-pending-state](https://github.com/datastaxdevs/shared-assets/blob/master/astra/dashboard-pending-1000-update.png?raw=true)

The status will change to `Active` when the database is ready, this will only take 2-3 minutes. You will also receive an email when it is ready.

**👁️ Walkthrough**

![db-creation-walkthrough](images/tutorials/astra-create-db.gif?raw=true)

[🏠 Back to Table of Contents](#0-table-of-contents)

## 2. Create Astra Token

We need to create a **token** that we will use as our credentials.

✅ **Step 2a: Generate Token**

Following the [Manage Application Tokens docs](https://docs.datastax.com/en/astra/docs/manage-application-tokens.html) to create a token with `Database Administrator` roles.

- Go the `Organization Settings`

- Go to `Token Management`

- Pick the role `Database Admnistrator` on the select box

- Click Generate token

**👁️ Walkthrough**

![image](images/tutorials/astra-create-token.gif?raw=true)

This is what the token page looks like. You can now download the values as a CSV. We will need those values, but you can also keep this window open for use later.

![image](images/tutorials/astra-token.png?raw=true)

Notice the clipboard icon at the end of each value.

- `Client Id:` We will use it as a _username_ to contact Cassandra in the field `quarkus.cassandra.auth.username` in the `application.properties` file.

- `Client Secret:` We will use it as a _password_ to contact Cassandra in the field `quarkus.cassandra.auth.password` in the `application.properties` file.

- `Token:` It can be used as an api token key to interact with APIs. We won't use it in the workshop today.

To learn more about roles, tokens, etc. you can lok at [this video.](https://www.youtube.com/watch?v=TUTCLsBuUd4)

> **Note: Make sure you don't close the window accidentally or otherwise - if you close this window before you copy the values, the application token is lost forever. They won't be available later for security reasons. You'll have to create a new application token.**

We are now set with the database and credentials. Let's start coding with Quarkus!

[🏠 Back to Table of Contents](#0-table-of-contents)

## 3. Launch Gitpod

[Gitpod](https://www.gitpod.io/) is an IDE 100% online based on [VS Code](https://github.com/gitpod-io/vscode/blob/gp-code/LICENSE.txt?lang=en-US). To initialize your environment simply click on the button below
_(CTRL + Click to open in new tab)_ You will be asked for you github account, as needed.

[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/datastaxdevs/quarkus-astra-intro-demo)

**👁️ Expected output**

_The screenshot may be slightly different based on your default skin and a few edits in the read me._

![gitpod](images/tutorials/gitpod-01-home-plain.png?raw=true)

**That's it.** Gitpod provides all tools we will need today including `maven` and exposing port `8080`, ports `5005` which is used for debugging, etc. 

**You may safely ignore the error output at the end of the terminal window.**

**👁️ Expected output**

![image](images/tutorials/gitpod-quarkus-1.jpg?raw=true)

Although GitPod terminal might seem to be available, the setup might still be ongoing. Wait for a few minutes before entering commands in the GitPod terminal window.

[🏠 Back to Table of Contents](#table-of-contents)

## 4. Know your gitpod

Take a moment to read this entire section since it'll help you with the rest of the workshop as you'll be spending most of your time in Gitpod. If you're familiar with Gitpod, you can easily skip this entire section.

The extreme left side has the explorer view(1). The top left, middle to right is where you'll be editing files(2), etc. and the bottom left, middle to right is what we will refer to as the Gitpod terminal window(3) as shown below.

**👁️ Expected output**

![gitpod](images/tutorials/gitpod-01-home-annotated.png?raw=true)


You can always get back to the file explorer view whenever by clicking on the hamburger menu on the top left followed by `View` and `Explorer` as shown below.

![gitpod](images/tutorials/Filexplorer0.png?raw=true)

✅ **Step 4a: Know your public URL**

The workshop application has opened with an ephemeral URL. To know the URL where your application endpoint will be exposed you can run the following command in the terminal after the build has completed. **Please note this URL and open this up in a new browser window as shown below**.

```bash
gp url 8080
```

**👁️ Expected output**

![gitpod](images/tutorials/gitpod-02-url.png?raw=true)


Although the application is not running yet, 
launch a new browser window (**don't close it for the rest of the workshop since you'll continually keep using this**. If you accidentally close it, just come back to this step. The browser will generate an error (due to application not running yet) which is fine for now as shown below.

**👁️ Expected output**

![gitpod](images/tutorials/newbrowser1.png?raw=true)

You may encounter the following at different steps and although this may not be applicable right away, the steps are included **in advance** and summarized here so that you can keep an eye out for it. Different paths and different environments might be slightly different although Gipod levels the playing field a bit.

You can allow cutting and pasting into the window by clicking on `Allow` as shown below.

![gitpod](images/tutorials/allow.png?raw=true)

Or allow ports to be opened by just exiting windows that are informational messages about ports like below.

![gitpod](images/tutorials/OpenPorts.png?raw=true)


[🏠 Back to Table of Contents](#0-table-of-contents)

## 5. Setup your application

To run the application you need to provide the credentials and identifier to the application.

Issue the following command from the Gitpod terminal window.

```
gp open src/main/resources/application.properties
```

![gitpod](images/tutorials/editapplicationproperties1.png?raw=true)


✅ **Step 5a: Enter 2 values from the token**


Enter the values of `Client Id` and `Client Secret` from values noted earlier for `astra-username` and `astra-password` respectively. The two lines with a TBD in comments is shown below.

![gitpod](images/tutorials/editapplicationproperties2.png?raw=true)


✅ **Step 5b: Download the secure connect bundle**


This next step is probably the most involved step in the entire workshop. The goal of this step is to get the customized connect bundle into Gitpod. One of the several ways of doing this is as follows.

Start with the [Astra DB dashboard](https://astra.datastax.com) and for the database workshops,

1. Click on `Connect` tab.
2. Click on Connect using a driver `Java`.
3. Click on `Download Bundle`.
4. Click on `Secure Connect Bundle` to be able to copy the link locally.

as shown below.

![gitpod](images/tutorials/secureconnectbundle1.png?raw=true)	

Locate the file locally in the finder/explorer window. Drag and drop the file into the Gitpod explorer window (on the left side, making sure that the cursor, indicating the drop is positioned in the Gitpod explorer window as shown below.

![gitpod](images/tutorials/secureconnectbundle3.png?raw=true)

In the Gitpod terminal window, verify that you dropped the right file and at the top level directory

```bash
ls -l secure-connect-workshops.zip
```

The file size should be roughly 12K otherwise something may have gone wrong in the process.

**👁️ Expected output**

```
-rw-r--r-- 1 gitpod gitpod 12231 Oct 26 00:15 secure-connect-workshops.zip
```

TADA your application is now configured we can finally play with some code.

[🏠 Back to Table of Contents](#0-table-of-contents)

## 6. Run some unit test(s)

The application is now set and you should be able to interact with your DB, although connecting to a "real" DB is not necessary in order to execute tests. The application's tests use the [Quarkus Cassandra Test Framework](https://github.com/datastax/cassandra-quarkus/tree/main/test-framework). Quarkus will automatically bootstrap a Cassandra Docker container and connect it to the application when the tests execute.

Under the covers, all interaction with Cassandra is implemented in Java through the `com.datastax.oss.driver.api.core.CqlSession`, part of the [DataStax Java Driver](https://docs.datastax.com/en/developer/java-driver/latest). Higher level frameworks like Quarkus, Spring, Spring Data, rely on this object. The [Quarkus Cassandra Test Framework](https://github.com/datastax/cassandra-quarkus/tree/main/test-framework) makes sure its available to the tests.

Let's run this unit test in the Gitpod terminal window.

```bash
./mvnw test -Dtest=com.datastaxdev.todo.rest.TodoResourceTests
```

**👁️ Expected output**

```
Picked up JAVA_TOOL_OPTIONS:  -Xmx3435m
[INFO] Scanning for projects...
[INFO] 
[INFO] ---------< quarkus-astra-intro-demo:quarkus-astra-intro-demo >----------
[INFO] Building quarkus-astra-intro-demo 0.01
[INFO] --------------------------------[ jar ]---------------------------------
[INFO] 
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ quarkus-astra-intro-demo ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 24 resources
[INFO] 
[INFO] --- quarkus-maven-plugin:2.9.1.Final:generate-code (default) @ quarkus-astra-intro-demo ---
[INFO] 
[INFO] --- maven-compiler-plugin:3.8.1:compile (default-compile) @ quarkus-astra-intro-demo ---
[INFO] Nothing to compile - all classes are up to date
[INFO] 
[INFO] --- quarkus-maven-plugin:2.9.1.Final:generate-code-tests (default) @ quarkus-astra-intro-demo ---
[INFO] 
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ quarkus-astra-intro-demo ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 1 resource
[INFO] 
[INFO] --- maven-compiler-plugin:3.8.1:testCompile (default-testCompile) @ quarkus-astra-intro-demo ---
[INFO] Nothing to compile - all classes are up to date
[INFO] 
[INFO] --- maven-surefire-plugin:3.0.0-M5:test (default-test) @ quarkus-astra-intro-demo ---
[INFO] 
[INFO] -------------------------------------------------------
[INFO]  T E S T S
[INFO] -------------------------------------------------------
[ERROR] Picked up JAVA_TOOL_OPTIONS:  -Xmx3435m
[INFO] Running com.datastaxdev.todo.rest.TodoResourceTests
INFO  [org.jbo.threads] (main) JBoss Threads version 3.4.2.Final
INFO  [org.tes.uti.ImageNameSubstitutor] (main) Image name substitution will be performed by: DefaultImageNameSubstitutor (composite of 'ConfigurationFileImageNameSubstitutor' and 'PrefixingImageNameSubstitutor')
INFO  [org.tes.doc.DockerClientProviderStrategy] (pool-4-thread-1) Loaded org.testcontainers.dockerclient.UnixSocketClientProviderStrategy from ~/.testcontainers.properties, will try it first
INFO  [org.tes.doc.DockerClientProviderStrategy] (pool-4-thread-1) Found Docker environment with local Unix socket (unix:///var/run/docker.sock)
INFO  [org.tes.DockerClientFactory] (pool-4-thread-1) Docker host IP address is localhost
INFO  [org.tes.DockerClientFactory] (pool-4-thread-1) Connected to docker: 
  Server Version: 20.10.14
  API Version: 1.41
  Operating System: Ubuntu 20.04.4 LTS
  Total Memory: 64312 MB
INFO  [org.tes.uti.RegistryAuthLocator] (pool-4-thread-1) Failure when attempting to lookup auth config. Please ignore if you don't have images in an authenticated registry. Details: (dockerImageName: testcontainers/ryuk:0.3.3, configFile: /home/gitpod/.docker/config.json. Falling back to docker-java default behaviour. Exception message: /home/gitpod/.docker/config.json (No such file or directory)
INFO  [org.tes.uti.RyukResourceReaper] (pool-4-thread-1) Ryuk started - will monitor and terminate Testcontainers containers on JVM exit
INFO  [org.tes.DockerClientFactory] (pool-4-thread-1) Checking the system...
INFO  [org.tes.DockerClientFactory] (pool-4-thread-1) ✔︎ Docker server version should be at least 1.6.0
INFO  [org.tes.DockerClientFactory] (pool-4-thread-1) ✔︎ Docker environment should have more than 2GB free disk space
INFO  [com.dat.oss.qua.tes.CassandraTestResource] (pool-4-thread-1) Container cassandra:latest starting...
INFO  [🐳 [cassandra:latest]] (pool-4-thread-1) Creating container for image: cassandra:latest
INFO  [🐳 [cassandra:latest]] (pool-4-thread-1) Container cassandra:latest is starting: 001d53b387a0a5861466621f6828e753036bec4d9958562ba24a934442c430e5
INFO  [com.dat.dri.core] (ducttape-0) DataStax Java driver 3.7.1 for Apache Cassandra
INFO  [com.dat.dri.cor.GuavaCompatibility] (ducttape-0) Detected Guava >= 19 in the classpath, using modern compatibility layer
INFO  [com.dat.dri.cor.ClockFactory] (ducttape-0) Using native clock to generate timestamps.
INFO  [com.dat.dri.cor.NettyUtil] (ducttape-0) Did not find Netty's native epoll transport in the classpath, defaulting to NIO.
INFO  [com.dat.dri.cor.ClockFactory] (ducttape-0) Using native clock to generate timestamps.
INFO  [com.dat.dri.cor.ClockFactory] (ducttape-0) Using native clock to generate timestamps.
WARN  [com.dat.dri.cor.Cluster] (ducttape-0) You listed localhost/127.0.0.1:49156 in your contact points, but it wasn't found in the control host's system.peers at startup
INFO  [com.dat.dri.cor.pol.DCAwareRoundRobinPolicy] (ducttape-0) Using data-center name 'datacenter1' for DCAwareRoundRobinPolicy (if this is incorrect, please provide the correct datacenter name with DCAwareRoundRobinPolicy constructor)
INFO  [com.dat.dri.cor.Cluster] (ducttape-0) New Cassandra host localhost/0:0:0:0:0:0:0:1:49156 added
INFO  [🐳 [cassandra:latest]] (pool-4-thread-1) Container cassandra:latest started in PT9.049449S
INFO  [org.tes.ext.ScriptUtils] (pool-4-thread-1) Executing database script from init_script.cql
INFO  [com.dat.dri.cor.ClockFactory] (pool-4-thread-1) Using native clock to generate timestamps.
WARN  [com.dat.dri.cor.Cluster] (pool-4-thread-1) You listed localhost/127.0.0.1:49156 in your contact points, but it wasn't found in the control host's system.peers at startup
INFO  [com.dat.dri.cor.pol.DCAwareRoundRobinPolicy] (pool-4-thread-1) Using data-center name 'datacenter1' for DCAwareRoundRobinPolicy (if this is incorrect, please provide the correct datacenter name with DCAwareRoundRobinPolicy constructor)
INFO  [com.dat.dri.cor.Cluster] (pool-4-thread-1) New Cassandra host localhost/0:0:0:0:0:0:0:1:49156 added
INFO  [org.tes.ext.ScriptUtils] (pool-4-thread-1) Executed database script from init_script.cql in 2132 ms.
INFO  [com.dat.dri.cor.ClockFactory] (pool-4-thread-1) Using native clock to generate timestamps.
WARN  [com.dat.dri.cor.Cluster] (pool-4-thread-1) You listed localhost/0:0:0:0:0:0:0:1:49156 in your contact points, but it wasn't found in the control host's system.peers at startup
INFO  [com.dat.dri.cor.pol.DCAwareRoundRobinPolicy] (pool-4-thread-1) Using data-center name 'datacenter1' for DCAwareRoundRobinPolicy (if this is incorrect, please provide the correct datacenter name with DCAwareRoundRobinPolicy constructor)
INFO  [com.dat.dri.cor.Cluster] (pool-4-thread-1) New Cassandra host localhost/127.0.0.1:49156 added
INFO  [com.dat.oss.qua.tes.CassandraTestResource] (pool-4-thread-1) Container cassandra:latest listening on 127.0.0.1:49156 (inferred local DC: datacenter1)
WARN  [io.qua.config] (main) Unrecognized configuration key "quarkus.kubernetes.service-type" was provided; it will be ignored; verify that the dependency extension for this configuration is set or that you did not make a typo
WARN  [io.qua.config] (main) Unrecognized configuration key "quarkus.kubernetes.env.secrets" was provided; it will be ignored; verify that the dependency extension for this configuration is set or that you did not make a typo
INFO  [com.dat.oss.qua.run.int.qua.CassandraClientRecorder] (main) Enabling Cassandra metrics using Micrometer.
INFO  [com.dat.oss.qua.run.int.qua.CassandraClientStarter] (main) Eagerly initializing Quarkus Cassandra client.
INFO  [com.dat.oss.dri.int.cor.DefaultMavenCoordinates] (main) DataStax Java driver for Apache Cassandra(R) (com.datastax.oss:java-driver-core) version 4.14.0
INFO  [com.dat.oss.dri.int.cor.tim.Clock] (vert.x-eventloop-thread-0) Using native clock for microsecond precision
INFO  [com.dat.tod.res.TodoResource] (main) **** Table created true****
INFO  [io.quarkus] (main) quarkus-astra-intro-demo 0.01 on JVM (powered by Quarkus 2.9.1.Final) started in 16.146s. Listening on: http://localhost:41159
INFO  [io.quarkus] (main) Profile test activated. 
INFO  [io.quarkus] (main) Installed features: [cassandra-client, cdi, micrometer, resteasy-reactive, resteasy-reactive-jackson, smallrye-context-propagation, smallrye-health, smallrye-openapi, swagger-ui, vertx]
[INFO] Tests run: 8, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 19.304 s - in com.datastaxdev.todo.rest.TodoResourceTests
INFO  [com.dat.oss.qua.run.int.qua.CassandraClientRecorder] (main) Closing Quarkus Cassandra session.
INFO  [io.quarkus] (main) quarkus-astra-intro-demo stopped in 0.039s
[INFO] 
[INFO] Results:
[INFO] 
[INFO] Tests run: 8, Failures: 0, Errors: 0, Skipped: 0
[INFO] 
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  25.096 s
[INFO] Finished at: 2022-05-16T18:22:07Z
[INFO] ------------------------------------------------------------------------
```

You can see from the output that the `cassandra:latest` Docker container was started before the tests were executed.

Verfiy that the table got created with the following command

```bash
./mvnw test -Dcom.datastaxdev.todo.rest.TodoResourceTests | grep -i table
```

**👁️ Expected output**

```
INFO  [com.dat.tod.res.TodoResource] (main) **** Table created true****
```

> You will likely see that output repeated more than once.

Although a significant strength of the Quarkus platform is it's [continuous testing](https://quarkus.io/guides/continuous-testing) capabilities, we will skip tests going foraward and focus on the other capabilities of the platform (perhaps, another workshop sometime focussed mainly on testing capabilities, assuming there is enough demand).

[🏠 Back to Table of Contents](#0-table-of-contents)

## 7. Quarkus Dev Mode

Before we get started let's check that the Graal VM is available.

```
echo $GRAALVM_HOME
```

**👁️ Expected output**

```
/home/gitpod/.sdkman/candidates/java/current
```

✅ **Step 7a: Begin dev**

In the Gitpod terminal window, We will begin the inner loop journey in dev mode using the following command.


```bash
./mvnw quarkus:dev -DskipTests
```
**👁️ Expected output**

```
__  ____  __  _____   ___  __ ____  ______ 
 --/ __ \/ / / / _ | / _ \/ //_/ / / / __/ 
 -/ /_/ / /_/ / __ |/ , _/ ,< / /_/ /\ \   
--\___\_\____/_/ |_/_/|_/_/|_|\____/___/   
WARN  [io.qua.config] (Quarkus Main Thread) Unrecognized configuration key "quarkus.kubernetes.service-type" was provided; it will be ignored; verify that the dependency extension for this configuration is set or that you did not make a typo

WARN  [io.qua.config] (Quarkus Main Thread) Unrecognized configuration key "quarkus.kubernetes.env.secrets" was provided; it will be ignored; verify that the dependency extension for this configuration is set or that you did not make a typo
INFO  [com.dat.oss.qua.run.int.qua.CassandraClientRecorder] (Quarkus Main Thread) Enabling Cassandra metrics using Micrometer.
INFO  [com.dat.oss.qua.run.int.qua.CassandraClientStarter] (Quarkus Main Thread) Eagerly initializing Quarkus Cassandra client.
INFO  [com.dat.oss.dri.int.cor.DefaultMavenCoordinates] (Quarkus Main Thread) DataStax Java driver for Apache Cassandra(R) (com.datastax.oss:java-driver-core) version 4.14.0
INFO  [com.dat.oss.dri.int.cor.tim.Clock] (vert.x-eventloop-thread-0) Using native clock for microsecond precision
INFO  [com.dat.oss.dri.int.cor.ses.DefaultSession] (vert.x-eventloop-thread-9) [s0] Negotiated protocol version V4 for the initial contact point, but cluster seems to support V5, keeping the negotiated version
INFO  [com.dat.tod.res.TodoResource] (Quarkus Main Thread) **** Table created true****
INFO  [io.quarkus] (Quarkus Main Thread) quarkus-astra-intro-demo 0.01 on JVM (powered by Quarkus 2.9.1.Final) started in 7.383s. Listening on: http://localhost:8080
INFO  [io.quarkus] (Quarkus Main Thread) Profile dev activated. Live Coding activated.
INFO  [io.quarkus] (Quarkus Main Thread) Installed features: [cassandra-client, cdi, micrometer, resteasy-reactive, resteasy-reactive-jackson, smallrye-context-propagation, smallrye-health, smallrye-openapi, swagger-ui, vertx]

--
Tests paused
Press [r] to resume testing, [o] Toggle test output, [:] for the terminal, [h] for more options>
```
✅ **Step 7b: Explore Quarkus dev mode**

Typing `h` in the terminal window should bring up the following

**👁️ Expected output**

```
The following commands are currently available:

== Continuous Testing

[r] - Resume testing
[o] - Toggle test output (disabled)

== Exceptions

[x] - Opens last exception in IDE (None)

== HTTP

[w] - Open the application in a browser
[d] - Open the Dev UI in a browser

== System

[s] - Force restart
[i] - Toggle instrumentation based reload (disabled)
[l] - Toggle live reload (enabled)
[j] - Toggle log levels (INFO)
[h] - Shows this help
[:] - Enters terminal mode
[q] - Quits the application

--
Tests paused
Press [r] to resume testing, [o] Toggle test output, [:] for the terminal, [h] for more options>
```

You can try the different options available.

Note that you may have to allow popups from `gitpod.io` as shown below.

![gitpod](images/tutorials/quarkus-dev-0.png?raw=true)

Try connecting to the application in a browser by pressing the `w` key as indicated above. This should bring up the application as below.

> **NOTE:** This may not actually work in Gitpod because of the way Gitpod handles links. If the page doesn't open for you simply go to the browser tab you opened back in step 4a.

**👁️ Expected output**

![gitpod](images/tutorials/quarkus-dev-1.png?raw=true)

✅ **Step 7c: Hot reload**
Now, let's go ahead and make an illustrative change.

Navigate to the file `src/main/java/resources/META-INF/resources/index.html` and change the `data-ribbon` entry from "Fork me on Github" to "Fork me today on Github."

Next, refresh the browser page. You should immediately see the update. This hot reloading feature of Quarkus works for more than just static content too!

**👁️ Expected output**

![gitpod](images/tutorials/quarkus-dev-2.png?raw=true)

You could add a few entries to the "todo list" as shown below.

**👁️ Expected output**

![gitpod](images/tutorials/quarkus-dev-3.png?raw=true)

✅ **Step 7d: Quarkus dev metrics**

You can get Quarkus development metrics and so on hitting the `d` key as indicated in the help and this should bring up a window that looks like below.

> **NOTE:** Again, because of the way Gitpod works, this may not bring up the window. If it doesn't, go back to the browser window containing the running application and append `/q/dev` to the URL.

**👁️ Expected output**

![gitpod](images/tutorials/quarkus-dev-4.png?raw=true)

> The `/q/dev` end point can be accessed using the URL below when the application is running in `dev` mode. You can use another `bash` tab in the gitpod terminal window to enter the commands while the application is running and switch between tabs as required.

```
echo $(gp url 8080)/q/dev
```

and the `openapi` spec is at

```
echo $(gp url 8080)/q/openapi
```

as shown below.

![gitpod](images/tutorials/quarkus-dev-5.png?raw=true)

You could continue developing your entire application without ever leaving dev mode! It will even handle refactorings and project dependency changes!

Let's out of development mode by hitting `q` or hitting `<Ctrl>+C`.

✅ **Step 7e: Quarkus smart dispatch**

Let's quickly examine the [Quarkus smart dispatching mechanism](https://quarkus.io/blog/resteasy-reactive-smart-dispatch). Let's open the `com.datastaxdev.todo.rest.TodoResource.java` class by executing the following in a terminal:

```
gp open src/main/java/com/datastaxdev/todo/rest/TodoResource.java
```

Notice some methods return [Mutiny reactive types](https://quarkus.io/guides/mutiny-primer) (i.e. `Uni` and `Multi`) while others return non-reactive types (i.e. `String`, `Response`, and `void`). Quarkus doesn't care whether you write reactive or blocking code. Quarkus will make sure reactive code runs on the I/O thread while blocking code will be moved off onto worker thread(s). As you can see, you can even mix and match within the same class!

This application was designed to illustrate both approaches. Which methods are implemented in which manner were randomly chosen.

[🏠 Back to Table of Contents](#0-table-of-contents)

## 8. Debugging

You can always get back to the file explorer view whenever by clicking on the hamburger menu on the top left followed by `View` and `Explorer` as shown below.

![gitpod](images/tutorials/Filexplorer0.png?raw=true)

Let's go ahead and hit `q` or `Ctrl+C` to exit out of the running application if you have not already.

Before we set a breakpoint and start debugging, let's explain the application's structure a bit. The `TodoResource` class (which you saw in the previous section) calls a `com.datastaxdev.todo.service.AstraService`. `AstraService` is an interface with 2 implementations:
1. `com.datastaxdev.todo.service.CqlSessionAstraService.java`
    - Uses the `com.datastax.oss.quarkus.runtime.api.session.QuarkusCqlSession` directly using CQL queries to implement the operations.
2. `com.datastaxdev.todo.service.MapperAstraService.java`
    - Uses the [DataStax Object Mapper](https://docs.datastax.com/en/developer/java-driver/latest/manual/mapper) and [Cassandra Entity Modeling](https://quarkus.io/guides/cassandra#creating-the-data-model-and-data-access-objects) to model entities as Java objects, greatly simplifying the application's data access layer by sparing you the hassle of writing CQL queries by hand. This is conceptually similar to how [Hibernate](https://hibernate.org) works in the [JPA](https://docs.oracle.com/javaee/6/tutorial/doc/bnbpz.html) world.
    - Uses [Mapstruct](https://mapstruct.org) to easily convert between the `com.datastaxdev.todo.dao.TodoItem` dao entities and the `com.datastaxdev.todo.api.Todo` POJO used at the REST layer. See the `com.datastaxdev.todo.mapping.TodoMapper` class for details.

The application has a [build-time property](https://quarkus.io/guides/cdi-reference#enable_build_properties) (not overridable at runtime) called `astra-service.type`. Resolving dependency injection at build time is one of the key benefits of Quarkus, allowing Quarkus applications to be super small and memory-performant. 

By default, if this property is undefined _OR_ has the value `cql-session`, `com.datastaxdev.todo.service.CqlSessionAstraService` will be injected as the implementation for `com.datastaxdev.todo.service.AstraService`.

If, at build time, `astra-service.type=dao`, then `com.datastaxdev.todo.service.MapperAstraService` will be used instead.

The `com.datastaxdev.todo.config.AstraConfig` class contains everything needed for reading the `astra-service.type` flag at build time and injecting the appropriate `com.datastaxdev.todo.service.AstraService` implementation.

There is a full suite of unit tests that cover both implementations! See `src/test/java/com/datastaxdev/todo/service/CqlSessionAstraServiceTests.java` and  `src/test/java/com/datastaxdev/todo/service/MapperAstraServiceTests.java` for details.

> If you'd like to play around with the mapper implementation, open `src/main/resources/application.properties` and un-comment the line `#astra-service.type=dao`.

Since the application, by default, uses the `com.datastaxdev.todo.service.CqlSessionAstraService` implementation, we'll use that implementation for setting the breakpoint.

✅ **Step 8a: Set breakpoint**

Now issue the following command to open up the file where we will subsequently set a breakpoint to be hit whenever we create a new todo item.

```
gp open src/main/java/com/datastaxdev/todo/service/CqlSessionAstraService.java
```

Now navigate to line 52 and set a breakpoint by clicking to the left of the line number 52 and you'll see a stop sign as shown below.

![gitpod](images/tutorials/debug1.png?raw=true)

Re-run the application with the following command and we will debug it live.

```bash
./mvnw quarkus:dev
```

✅ **Step 8b: Start debugging**

First, click on the debug button on the left towards top.

Then, click on the arrow on the top left to start debugging as shown below.

![gitpod](images/tutorials/debug2.png?raw=true)

Notice debug related information populate in the left side of the window as shown below.

![gitpod](images/tutorials/debug3.png?raw=true)

> If for some reason the debugger doesn't attach, there may be some weirdness going on with the Gitpod IDE. Refreshing the browser tab containing the workspace or restarting the workspace will usually fix it.

We will demonstrate debugging by fixing a todo item that was entered although there are more powerful features that you can try. 

Go to the new browser window you created and the application should be up and running. Hit enter after filling up a todo item as shown below. The application freezes and you see a red square that signals the breakpoint has been hit.

![gitpod](images/tutorials/debug4.png?raw=true)

Now that you hit the breakpoint, Cool! Let's go back to the Gitpod window and verify if the breakpoint was really hit. You should see something like below.

**👁️ Expected output**

![gitpod](images/tutorials/debug5.png?raw=true)

✅ **Step 8c: Use debugger features**

Now, fix the entry spelling in debug mode by clicking on the left(>) arrow of `todo`, double clicking on the `title` entry and entering `debugging`. Finally, hit arrow button in the top small middle pane in the center which will allow the application to continue as shown below.

![gitpod](images/tutorials/debug6.png?raw=true)

Go back to your browser and check the todo item that was added to the list. You should see that the updated entry that you fixed with a debug session is what's persisted.

Hit `q` or `Ctrl+C` in the GitPod terminal window to exit the application.

[🏠 Back to Table of Contents](#0-table-of-contents)

## 9. Packaging

✅ **Step 9a: Package**
You can package up the application with the command below.

```bash
./mvnw clean package -DskipTests
```

**👁️ Expected output**

```
Picked up JAVA_TOOL_OPTIONS:  -Xmx3435m
[INFO] Scanning for projects...
[INFO] 
[INFO] ---------< quarkus-astra-intro-demo:quarkus-astra-intro-demo >----------
[INFO] Building quarkus-astra-intro-demo 0.01
[INFO] --------------------------------[ jar ]---------------------------------
[INFO] 
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ quarkus-astra-intro-demo ---
[INFO] Deleting /workspace/quarkus-astra-intro-demo/target
[INFO] 
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ quarkus-astra-intro-demo ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 24 resources
[INFO] 
[INFO] --- quarkus-maven-plugin:2.9.1.Final:generate-code (default) @ quarkus-astra-intro-demo ---
[INFO] 
[INFO] --- maven-compiler-plugin:3.8.1:compile (default-compile) @ quarkus-astra-intro-demo ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 10 source files to /workspace/quarkus-astra-intro-demo/target/classes
[INFO] 
[INFO] --- quarkus-maven-plugin:2.9.1.Final:generate-code-tests (default) @ quarkus-astra-intro-demo ---
[INFO] 
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ quarkus-astra-intro-demo ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 1 resource
[INFO] 
[INFO] --- maven-compiler-plugin:3.8.1:testCompile (default-testCompile) @ quarkus-astra-intro-demo ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 6 source files to /workspace/quarkus-astra-intro-demo/target/test-classes
[INFO] 
[INFO] --- maven-surefire-plugin:3.0.0-M5:test (default-test) @ quarkus-astra-intro-demo ---
[INFO] Tests are skipped.
[INFO] 
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ quarkus-astra-intro-demo ---
[INFO] Building jar: /workspace/quarkus-astra-intro-demo/target/quarkus-astra-intro-demo-0.01.jar
[INFO] 
[INFO] --- quarkus-maven-plugin:2.9.1.Final:build (default) @ quarkus-astra-intro-demo ---
[WARNING] [io.quarkus.config] Unrecognized configuration key "quarkus.kubernetes.env.secrets" was provided; it will be ignored; verify that the dependency extension for this configuration is set or that you did not make a typo
[WARNING] [io.quarkus.config] Unrecognized configuration key "quarkus.kubernetes.service-type" was provided; it will be ignored; verify that the dependency extension for this configuration is set or that you did not make a typo
[INFO] [io.quarkus.deployment.QuarkusAugmentor] Quarkus augmentation completed in 2076ms
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  9.681 s
[INFO] Finished at: 2022-05-16T20:21:41Z
[INFO] ------------------------------------------------------------------------
```

✅ **Step 9b: Run**

You can run the recently packaged application as below.

```bash
java -jar ./target/quarkus-app/quarkus-run.jar
```

**👁️ Expected output**

```
Picked up JAVA_TOOL_OPTIONS:  -Xmx3435m
__  ____  __  _____   ___  __ ____  ______ 
 --/ __ \/ / / / _ | / _ \/ //_/ / / / __/ 
 -/ /_/ / /_/ / __ |/ , _/ ,< / /_/ /\ \   
--\___\_\____/_/ |_/_/|_/_/|_|\____/___/   
WARN  [io.qua.config] (main) Unrecognized configuration key "quarkus.kubernetes.service-type" was provided; it will be ignored; verify that the dependency extension for this configuration is set or that you did not make a typo
WARN  [io.qua.config] (main) Unrecognized configuration key "quarkus.kubernetes.env.secrets" was provided; it will be ignored; verify that the dependency extension for this configuration is set or that you did not make a typo
INFO  [com.dat.oss.qua.run.int.qua.CassandraClientRecorder] (main) Enabling Cassandra metrics using Micrometer.
INFO  [io.qua.sma.ope.run.OpenApiRecorder] (main) Default CORS properties will be used, please use 'quarkus.http.cors' properties instead
INFO  [com.dat.oss.qua.run.int.qua.CassandraClientStarter] (main) Eagerly initializing Quarkus Cassandra client.
INFO  [com.dat.oss.dri.int.cor.DefaultMavenCoordinates] (main) DataStax Java driver for Apache Cassandra(R) (com.datastax.oss:java-driver-core) version 4.14.0
INFO  [com.dat.oss.dri.int.cor.tim.Clock] (vert.x-eventloop-thread-0) Using native clock for microsecond precision
INFO  [com.dat.oss.dri.int.cor.ses.DefaultSession] (vert.x-eventloop-thread-9) [s0] Negotiated protocol version V4 for the initial contact point, but cluster seems to support V5, keeping the negotiated version
INFO  [com.dat.tod.res.TodoResource] (main) **** Table created true****
INFO  [io.quarkus] (main) quarkus-astra-intro-demo 0.01 on JVM (powered by Quarkus 2.9.1.Final) started in 3.882s. Listening on: http://0.0.0.0:8080
INFO  [io.quarkus] (main) Profile prod activated. 
INFO  [io.quarkus] (main) Installed features: [cassandra-client, cdi, micrometer, resteasy-reactive, resteasy-reactive-jackson, smallrye-context-propagation, smallrye-health, smallrye-openapi, swagger-ui, vertx]
```

You can play with the application from the new browser window you created in step 4.

Hit `Ctrl+C` in the GitPod terminal window to exit the application.

[🏠 Back to Table of Contents](#0-table-of-contents)

## 10. Containerizing

✅ **Step 10a: Adjust for containerization**

We are using the [Quarkus Jib container extension](https://quarkus.io/guides/container-image#jib) for easy containerization. Copy the secure connect bundle to the directory that Jib will create on the container as below. We take advantage of the property of Jib plugin which automaticaly includes the relevant artifacts from the `src/main/jib` sub-directory as part of the process -- we include the secure connect bundle to be able to connect to the Astra database.


```bash
mkdir -p src/main/jib/workspace/quarkus-astra-intro-demo/
cp secure-connect-workshops.zip src/main/jib/workspace/quarkus-astra-intro-demo/
```

✅ **Step 10b: Containerize**

Let's containerize the application with the following command.

```bash
./mvnw clean package -Dquarkus.container-image.build=true -DskipTests
```

Once complete, check that the image exists on your local repository with the following command:

```bash
docker images
```

**Expected Output**

```
REPOSITORY                 TAG       IMAGE ID       CREATED          SIZE
gitpod/quarkus-cassandra   0.01      77431983359a   26 seconds ago   216MB
```

✅ **Step 10c: Run the containerized image**

You can execute the recently generated containerized image with the following command.

```bash
docker run -i --rm -p 8080:8080 gitpod/quarkus-cassandra:0.01
```

Hit <Ctrl+C> in the GitPod terminal window to exit the application.

✅ **Step 10d: docker login**

Login to Dockerhub to be able to push containerized images and to be able for you and the rest of the world to pull them.

If you do not have a docker login credential you can skip this step and go right to [Native Image](#11-native-image)

```bash
docker login
```

**Expected Output**

```
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: ragsns
Password: 
WARNING! Your password will be stored unencrypted in /home/gitpod/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
```

Get the Docker ID as below.

```bash
docker system info | grep -E 'Username' || echo "you have not done a docker login(yet)"
DOCKER_LOGINID=$(docker system info | grep -E 'Username' | awk '{print $2}')
echo "Docker login ID that will be used: " "$DOCKER_LOGINID"
```

You should see an output of your Docker Login ID. If you do not see this repeat this step from the beginning.

```
Docker login ID that will be used:  ragsns
```

✅ **Step 10e: Push to Dockerhub**

Let's not only build the containerized image but also push it to DockerHub (**be sure to substitute the group with docker ID**) with the following command.

```bash
./mvnw clean package -Dquarkus.container-image.build=true -Dquarkus.container-image.push=true -Dquarkus.container-image.group=$DOCKER_LOGINID -DskipTests
```

**NOTE:** If the repo in your Docker Hub account didn't previously exist it will be created for you as a _private_ repo. You will need to go into your Docker Hub account and make the repo public.

In today's world of microservices and service meshes, it's all about deploying to Kubernetes. Quarkus gives us an easy way to do that!

**Step A**: Quarkus includes the [Kubernetes extension](https://quarkus.io/guides/deploying-to-kubernetes), allowing developers to deploy directly to Kubernetes and use Kubernetes `ConfigMap`s and `Secret`s as configuration sources. To use this update `pom.xml` with the following command in the Gitpod ternimal window as below.

```bash
./mvnw quarkus:add-extension -Dextensions="kubernetes"
```

and verify with the following command

```bash
git diff pom.xml
```

which should that the `io.quarkus:quarkus-kubernetes` dependency has been added.

```bash
diff --git a/pom.xml b/pom.xml
index 371e35e..4b842d8 100644
--- a/pom.xml
+++ b/pom.xml
+    <dependency>
+      <groupId>io.quarkus</groupId>
+      <artifactId>quarkus-kubernetes</artifactId>
+    </dependency>
```

**Step B**:
Ensure that `DOCKER_LOGINID` was set in the earlier step as below and the command should output the Docker login ID.

```
echo $DOCKER_LOGINID
```

Next, let's generate the containerized image with the secrets as below.

```
./mvnw clean package -Dquarkus.container-image.build=true -Dquarkus.container-image.push=false -Dquarkus.container-image.group=$DOCKER_LOGINID -DskipTests
```

Next, let's generate and push the containerized image with the secrets removed as below.

```
sed -i '/# TBD Below/,+4 d' ./target/classes/application.properties
./mvnw package -Dquarkus.container-image.build=true -Dquarkus.container-image.push=true -Dquarkus.container-image.group=$DOCKER_LOGINID -DskipTests
```

✅ **Step 10f: Stand up application in Kubernetes**

Now, you can deploy your application to Kubernetes. 

Hereafter, there are two paths -- steps prefixed with `Lens` OR steps prefixed with `Okteto` as summarized below.

|Steps prefixed with|Details| First step|
| ----------- | ----------- |-----|
|Lens| Install on Lens IDE|Lens.A
|Okteto| Sample Kubernetes provider|Okteto.A

If you're familiar with using `kubectl` command you can pick either. If you're not you're recommended to use Steps prefixed with `Lens`.

**Step Lens.A**: Download Lens

[Lens](https://k8slens.dev/) Lens provides the full situational awareness for everything that runes in Kubernetes. Lens provides its users with a simple graphical user interface that provides the necessary tools and resources to easily manage and monitor multiple Kubernetes cluster, applications all the resources within. 

[Download](https://docs.k8slens.dev/main/#downloading-lens) Lens on your local system.

**Step Lens.B**: This step is optional: You can either leverage a fully managed development cluster via Lens, or use your own Kubernetes distro. Sign up for dev clusters

You can create a Kubernetes cluster on the cloud by signing up for [Managed dev clusters](https://k8slens.dev/kubernetes.html) on Lens. Sample screen shots are shown below

![lens](images/tutorials/Lens01.png?raw=true)

Eventually you'll have a dev cluster on the cloud as shown below which is where we will deploy our workload(s). **Note**: You may need to provide a credit card for billing purposes to enable this feature.

![lens](images/tutorials/Lens02.png?raw=true)

**Step Lens.C**: Set up secrets

Start by clicking on `Config`, `Secrets` and `+` as shown below.

![lens](images/tutorials/Lenssecret01.png?raw=true)

Now add the secrets `astra-username` and `astra-password` as shown below.

![lens](images/tutorials/Lenssecret02.png?raw=true)

You can verify you entered the values correctly by hitting the button as shown below which will show the values in clear.

![lens](images/tutorials/Lenssecret03.png?raw=true)

**Step Lens.D**: Start the workload

We use the generated `kubernetes.yml` file to install the workload in Lens.

Click on `+` and `Create resource` as shown below.

![lens](images/tutorials/Lensresource01.png?raw=true)

We are going to cut-n-paste the contents from the Gitpod window into Lens.

Issue the following command in the Gitpod terminal window to look at the Kubernetes manifests that were automatically generated and applied to the cluster.

```
gp open target/kubernetes/kubernetes.yml
```

and cut-n-paste the contents into Lens as shown below and hit `Create` as shown below.

![lens](images/tutorials/Lensresource02.png?raw=true)

If you dig into `Pods` you should be see it running as shown below.

![lens](images/tutorials/Lensresource03.png?raw=true)

You can look through other Kubernetes artifacts which you can easily navigate via Lens.

**Step Lens.E**: Cleanup as below.

You can delete the service, deployment and secret using Lens.

You're now done with Lens deployment and can skip other deployment(s). 

**OR**

**Step Okteto.A**: Create a cluster

We've included some steps here using [okteto](https://www.okteto.com) (you can modify the steps below depending on your choice of provider).

Create a Kubernetes cluster. You can get one for free at [https://okteto.com](https://okteto.com) with your Github credentials.

**Step Okteto.B**: Download config file

Download the `config` file from [https://cloud.okteto.com/#/settings/setup](https://cloud.okteto.com/#/settings/setup) locally as shown below.

![okteto](images/tutorials/oktetoconfig1.png?raw=true)

**Step Okteto.C**: Drag and drop config into Gitpod

Now "drag and drop" it over to the gitpod window (similar to how we transferred the secure connect bundle) as shown below.

![okteto](images/tutorials/oktetoconfig2.png?raw=true)

**Step Okteto.D**: Setup config in Gitpod window

Use the transferred okteto config file with the following command in the Gitpod terminal window and verify

```bash
export KUBECONFIG=okteto-kube.config
kubectl config get-contexts
kubectl get all
```

If you get a message like `error: You must be logged in to the server (Unauthorized)` reauthorize and download the `okteto-kube.config` file again.

Since okteto only provides access to your namespace, you should see something like below and you won't be able to run other commands like you would normally with a cluster that you created.

```
No resources found in ragsns namespace.
```

**Step Okteto.E**: Setup secrets

The `application.properties` file is setup to use the Kubernetes secrets already (`quarkus.kubernetes.env.secrets=astra`). Setup the Kubernetes secrets as below from the`Client Id` and `Client Secret` respectively as we did earlier.

```
kubectl create secret generic astra --from-literal=astra-username=<Client Id> --from-literal=astra-password=<Client Secret>
```

Verify the screts are setup properly with the following commands.

```bash
kubectl get secret astra -o jsonpath="{.data.astra-username}" | base64 --decode
kubectl get secret astra -o jsonpath="{.data.astra-password}" | base64 --decode
```

The `kubernetes.yml` deployment file that will be generated in the next step will be setup to use the secrets.

**Step Okteto.F**: Push image

You may want to remove the actual values of `astra-username` and `astra-password` from the`application.properties` file as below before pushing the container image to a public registry.

```
sed -i '/# TBD Below/,+4 d' ./target/classes/application.properties
./mvnw package -Dquarkus.container-image.build=true -Dquarkus.container-image.push=true -Dquarkus.container-image.group=$DOCKER_LOGINID -Dquarkus.kubernetes.deploy=true -DskipTests
```

The result of this command will be that your application's container image is built, pushed to the registry, and then deployed on Kubernetes. The Quarkus Kubernetes extension generates all the necessary Kubernetes desriptors for you.

Issue the following command in the Gitpod terminal window to look at the Kubernetes manifests that were automatically generated and applied to the cluster.

```
gp open target/kubernetes/kubernetes.yml
```

You should see the following output which indicates the deployment and the service have been created. **You can ignore errors related to webhook.**

```
[INFO] [io.quarkus.container.image.jib.deployment.JibProcessor] Pushed container image xxx/quarkus-cassandra:0.01 (sha256:xxxxxx)

[INFO] [io.quarkus.kubernetes.deployment.KubernetesDeployer] Deploying to kubernetes server: https://a.b.c.d:443/ in namespace: xxxx.
[INFO] [io.quarkus.kubernetes.deployment.KubernetesDeployer] Applied: Service quarkus-cassandra.
[INFO] [io.quarkus.kubernetes.deployment.KubernetesDeployer] Applied: Deployment quarkus-cassandra.
[INFO] [io.quarkus.deployment.QuarkusAugmentor] Quarkus augmentation completed in 12904ms
```

Running the command

```bash
kubectl get all
```

should yield a different output as below.

```
NAME                                    READY   STATUS    RESTARTS   AGE
pod/quarkus-cassandra-5f4d69b8d-lx46l   1/1     Running   0          95s

NAME                        TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)   AGE
service/quarkus-cassandra   ClusterIP   10.154.219.218   <none>        80/TCP    96s

NAME                                READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/quarkus-cassandra   1/1     1            1           96s

NAME                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/quarkus-cassandra-5f4d69b8d   1         1         1       96s
```

You should be able to access the application from the endpoint provided by the [okteto console](https://cloud.okteto.com/#/spaces) and also be able to access the application logs as shown below.

![okteto](images/tutorials/oktetorunning1.png?raw=true)

Alternately, you can access the application provided by the gitpod URL like we have always been doing throughout the workshop by issuing the following command.

```
kubectl port-forward svc/quarkus-cassandra 8080:80 &
```

**Step Okteto.G**: Cleanup

You can stop the port forwarding by deleting the background job as below.

```
kill -9 %1
``` 

You can go ahead and get rid of the application as well with the following command from the Gitpod terminal window.

```
kubectl delete all --all
```

and you should see the following output.

```
pod "quarkus-cassandra-886d9f8b9-h7tw5" deleted
service "quarkus-cassandra" deleted
deployment.apps "quarkus-cassandra" deleted
```

Additionally, you should delete the `Secret` you created.

```
kubectl delete secret astra
```

If the public docker image contains the credentials to be able to access the database, it's a good idea to delete the docker image [from docker hub](http://hub.docker.com) right away, anyway.

[🏠 Back to Table of Contents](#0-table-of-contents)

## 11. Native Image

Finally, Quarkus can build a native executable image with the help of the GraalVM that was pre-installed with a simple command as below. **Get yourself some coffee** or water as this will take almost 10 minutes but exceuting this image will be super fast with minimal startup time since it does not depend on the JVM.

✅ **Step 11a: Generating Native Image**

```bash
./mvnw clean package -Pnative -DskipTests
```

**Expected output:**
```
Picked up JAVA_TOOL_OPTIONS:  -Xmx3435m
[INFO] Scanning for projects...
[INFO] 
[INFO] ---------< quarkus-astra-intro-demo:quarkus-astra-intro-demo >----------
[INFO] Building quarkus-astra-intro-demo 0.01
[INFO] --------------------------------[ jar ]---------------------------------
[INFO] 
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ quarkus-astra-intro-demo ---
[INFO] Deleting /workspace/quarkus-astra-intro-demo/target
[INFO] 
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ quarkus-astra-intro-demo ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 24 resources
[INFO] 
[INFO] --- quarkus-maven-plugin:2.9.1.Final:generate-code (default) @ quarkus-astra-intro-demo ---
[INFO] 
[INFO] --- maven-compiler-plugin:3.8.1:compile (default-compile) @ quarkus-astra-intro-demo ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 10 source files to /workspace/quarkus-astra-intro-demo/target/classes
[INFO] 
[INFO] --- quarkus-maven-plugin:2.9.1.Final:generate-code-tests (default) @ quarkus-astra-intro-demo ---
[INFO] 
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ quarkus-astra-intro-demo ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 1 resource
[INFO] 
[INFO] --- maven-compiler-plugin:3.8.1:testCompile (default-testCompile) @ quarkus-astra-intro-demo ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 6 source files to /workspace/quarkus-astra-intro-demo/target/test-classes
[INFO] 
[INFO] --- maven-surefire-plugin:3.0.0-M5:test (default-test) @ quarkus-astra-intro-demo ---
[INFO] Tests are skipped.
[INFO] 
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ quarkus-astra-intro-demo ---
[INFO] Building jar: /workspace/quarkus-astra-intro-demo/target/quarkus-astra-intro-demo-0.01.jar
[INFO] 
[INFO] --- quarkus-maven-plugin:2.9.1.Final:build (default) @ quarkus-astra-intro-demo ---
[INFO] [io.quarkus.deployment.pkg.steps.JarResultBuildStep] Building native image source jar: /workspace/quarkus-astra-intro-demo/target/quarkus-astra-intro-demo-0.01-native-image-source-jar/quarkus-astra-intro-demo-0.01-runner.jar
[INFO] Checking for existing resources in: /workspace/quarkus-astra-intro-demo/src/main/kubernetes.
[INFO] [io.quarkus.deployment.pkg.steps.NativeImageBuildStep] Building native image from /workspace/quarkus-astra-intro-demo/target/quarkus-astra-intro-demo-0.01-native-image-source-jar/quarkus-astra-intro-demo-0.01-runner.jar
[INFO] [io.quarkus.deployment.pkg.steps.NativeImageBuildStep] Running Quarkus native-image plugin on GraalVM 22.0.0.2 Java 11 CE (Java Version 11.0.14+9-jvmci-22.0-b05)
[INFO] [io.quarkus.deployment.pkg.steps.NativeImageBuildRunner] /home/gitpod/.sdkman/candidates/java/current/bin/native-image -J-Dsun.nio.ch.maxUpdateArraySize=100 -J-Djava.util.logging.manager=org.jboss.logmanager.LogManager -J-Dvertx.logger-delegate-factory-class-name=io.quarkus.vertx.core.runtime.VertxLogDelegateFactory -J-Dvertx.disableDnsResolver=true -J-Dio.netty.leakDetection.level=DISABLED -J-Dio.netty.allocator.maxOrder=3 -J-Duser.language=en -J-Duser.country=US -J-Dfile.encoding=UTF-8 -H:-ParseOnce -J--add-exports=java.security.jgss/sun.security.krb5=ALL-UNNAMED -J--add-opens=java.base/java.text=ALL-UNNAMED -H:InitialCollectionPolicy=com.oracle.svm.core.genscavenge.CollectionPolicy\$BySpaceAndTime -H:+JNI -H:+AllowFoldMethods -J-Djava.awt.headless=true -H:FallbackThreshold=0 -H:+ReportExceptionStackTraces -H:-AddAllCharsets -H:EnableURLProtocols=http,https -H:NativeLinkerOption=-no-pie -H:-UseServiceLoaderFeature -H:+StackTrace quarkus-astra-intro-demo-0.01-runner -jar quarkus-astra-intro-demo-0.01-runner.jar
Picked up JAVA_TOOL_OPTIONS:  -Xmx3435m
========================================================================================================================
GraalVM Native Image: Generating 'quarkus-astra-intro-demo-0.01-runner'...
========================================================================================================================
[1/7] Initializing...                                                                                    (4.9s @ 0.42GB)
 Version info: 'GraalVM 22.0.0.2 Java 11 CE'
 3 user-provided feature(s)
  - io.quarkus.runner.AutoFeature
  - io.quarkus.runtime.graal.DisableLoggingAutoFeature
  - io.quarkus.runtime.graal.ResourcesFeature
[2/7] Performing analysis...  [20:33:32,360 INFO  [com.dat.oss.qua.run.int.qua.CassandraClientRecorder] Enabling Cassandra metrics using Micrometer.
20:33:36,471 INFO  [com.dat.oss.dri.int.cor.DefaultMavenCoordinates] DataStax Java driver for Apache Cassandra(R) (com.datastax.oss:java-driver-core) version 4.14.0
**********]                                                              (31.7s @ 2.17GB)
  14,155 (95.32%) of 14,850 classes reachable
  20,586 (66.60%) of 30,912 fields reachable
  69,696 (82.00%) of 84,993 methods reachable
     482 classes,    39 fields, and   850 methods registered for reflection
      68 classes,    88 fields, and    54 methods registered for JNI access
[3/7] Building universe...                                                                               (2.0s @ 2.69GB)
[4/7] Parsing methods...      [***]                                                                      (5.3s @ 1.81GB)
[5/7] Inlining methods...     [*****]                                                                    (4.7s @ 3.18GB)
[6/7] Compiling methods...    [*****]                                                                   (22.6s @ 3.42GB)
[7/7] Creating image...                                                                                  (4.3s @ 2.54GB)
  25.64MB (36.89%) for code area:   46,599 compilation units
  37.07MB (53.33%) for image heap:   9,899 classes and 350,222 objects
   6.80MB ( 9.78%) for other data
  69.50MB in total
------------------------------------------------------------------------------------------------------------------------
Top 10 packages in code area:                               Top 10 object types in image heap:
   1.67MB sun.security.ssl                                    19.83MB byte[] for general heap data
   1.03MB java.util                                            3.40MB java.lang.Class
 686.94KB com.sun.crypto.provider                              2.65MB java.lang.String
 531.52KB io.quarkus.runtime.generated                         2.21MB byte[] for java.lang.String
 490.01KB sun.security.x509                                  627.75KB java.util.HashMap$Node
 458.87KB java.util.concurrent                               581.63KB java.util.LinkedHashMap
 446.33KB io.netty.buffer                                    534.65KB com.oracle.svm.core.util.LazyFinalReference
 426.93KB com.oracle.svm.core.reflect                        526.91KB java.lang.String[]
 425.74KB com.typesafe.config.impl                           439.28KB byte[] for method metadata
 379.99KB java.lang                                          427.69KB c.o.s.c.h.DynamicHub$$Lambda$~31d9af6a7fe68cfc2a1f
      ... 661 additional packages                                 ... 3407 additional object types
                                           (use GraalVM Dashboard to see all)
------------------------------------------------------------------------------------------------------------------------
                        6.2s (7.7% of total time) in 40 GCs | Peak RSS: 5.21GB | CPU load: 4.98
------------------------------------------------------------------------------------------------------------------------
Produced artifacts:
 /workspace/quarkus-astra-intro-demo/target/quarkus-astra-intro-demo-0.01-native-image-source-jar/quarkus-astra-intro-demo-0.01-runner (executable)
 /workspace/quarkus-astra-intro-demo/target/quarkus-astra-intro-demo-0.01-native-image-source-jar/quarkus-astra-intro-demo-0.01-runner.build_artifacts.txt
========================================================================================================================
Finished generating 'quarkus-astra-intro-demo-0.01-runner' in 1m 20s.
[INFO] [io.quarkus.deployment.pkg.steps.NativeImageBuildRunner] objcopy --strip-debug quarkus-astra-intro-demo-0.01-runner
[INFO] [io.quarkus.deployment.QuarkusAugmentor] Quarkus augmentation completed in 83782ms
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  01:29 min
[INFO] Finished at: 2022-05-16T20:34:47Z
[INFO] ------------------------------------------------------------------------
```

✅ **Step 11b: Running Native Image**

Run the native image with the following command:

```bash
./target/quarkus-astra-intro-demo-0.01-runner
```

```
__  ____  __  _____   ___  __ ____  ______ 
 --/ __ \/ / / / _ | / _ \/ //_/ / / / __/ 
 -/ /_/ / /_/ / __ |/ , _/ ,< / /_/ /\ \   
--\___\_\____/_/ |_/_/|_/_/|_|\____/___/   
INFO  [io.qua.sma.ope.run.OpenApiRecorder] (main) Default CORS properties will be used, please use 'quarkus.http.cors' properties instead
INFO  [com.dat.oss.qua.run.int.qua.CassandraClientStarter] (main) Eagerly initializing Quarkus Cassandra client.
INFO  [com.dat.oss.dri.int.cor.os.Native] (vert.x-eventloop-thread-0) Using Graal-specific native functions
INFO  [com.dat.oss.dri.int.cor.tim.Clock] (vert.x-eventloop-thread-0) Using native clock for microsecond precision
INFO  [com.dat.oss.dri.int.cor.ses.DefaultSession] (vert.x-eventloop-thread-9) [s0] Negotiated protocol version V4 for the initial contact point, but cluster seems to support V5, keeping the negotiated version
INFO  [com.dat.tod.res.TodoResource] (main) **** Table created true****
INFO  [io.quarkus] (main) quarkus-astra-intro-demo 0.01 native (powered by Quarkus 2.9.1.Final) started in 2.273s. Listening on: http://0.0.0.0:8080
INFO  [io.quarkus] (main) Profile prod activated. 
INFO  [io.quarkus] (main) Installed features: [cassandra-client, cdi, kubernetes, micrometer, resteasy-reactive, resteasy-reactive-jackson, smallrye-context-propagation, smallrye-health, smallrye-openapi, swagger-ui, vertx]
```

Notice the fast startup time since the image is running as a native image.

Hit `Ctrl+C` in the GitPod terminal window to exit the application.

[🏠 Back to Table of Contents](#0-table-of-contents)

## 12. Homework

<img src="images/tutorials/badge.png?raw=true" width="200" align="right" />

Don't forget to complete your upgrade and get your verified skill badge! Finish and submit your homework! You have 2 options (option A or Option B). Pick whichever works for you.

Option A. Complete the practice steps from this repository as described above (including optional steps 10d and 10e) and deploy to a Kubernetes cluster.  Make screenshots of the deployment to a Kubernetes cluster.

Option B: Learn more about Quarkus and do some development with https://github.com/datastax/cassandra-quarkus. Send a screenshot of the working "Fruits application" with the following entry "Jackfruit" and the correpsonding description as "King/Queen of fruits".

3. Submit your homework [here](https://docs.google.com/forms/d/e/1FAIpQLSd3hMHgHURsNGq2-jDAIr-XPLJGHqaRAm9CAMkYveybuYfBRw/viewform)

That's it, you are done! Expect an email next week!

[🏠 Back to Table of Contents](#0-table-of-contents)

## 13. The END

Congratulations, your made it to the END of the show.


**🧑🏻‍🤝‍🧑🏽 Let's get in touch**

| ![B](images/tutorials/rags.png)                           |
| ---------------------------------------------------------- |
| Rags Srinivas <br>[@ragsns](https://github.com/ragsns) |

[🏠 Back to Table of Contents](#0-table-of-contents)
---

[![thankyou](images/tutorials/thankyou.gif)]()
