---
menutitle: Introduction to scripting
title: Introduction to Gatling scripting
description: "Get started with Gatling: install, write your first load test, and execute it."
lead: Learn how to get started with Gatling and create a Gatling simulation.
date: 2023-12-16T18:30:56+02:00
lastmod: 2023-12-16T21:30:56+02:00
---

{{< alert warning >}}
This guide is intended for Gatling versions `{{< var gatlingVersion >}}` and later.
{{< /alert >}}

Gatling is a highly flexible load-testing platform. You can write load tests in Java, Kotlin, and Scala or use our [no-code feature](https://gatling.io/features/no-code-generator/) with Gatling Enterprise. In this guide, we cover a "Hello world"-style example of how to:

 - [install and setup your local dev environment](#install-gatling),
 - [write your first simulation](#simulation-construction),
 - [run your simulation locally](#run-the-simulation-locally), 
 - [run a simulation on Gatling Enterprise Cloud](#run-the-simulation-on-gatling-enterprise-cloud).

{{< alert tip >}}
Join the [Gatling Community Forum](https://community.gatling.io) to discuss load testing with other users. Please try to find answers in the documentation before asking for help.
{{< /alert >}}

## Setup

This section guides you through installation and setting up your developer environment. Gatling has a lot of optionalities, including:

- build tools,
- CI/CD integrations,
- Java, Kotlin, and Scala SDKs

This guide uses Java and the Gatling bundle. Gatling recommends that developers use the Java SDK unless they are already experienced with Scala or Kotlin. Java is widely taught in CS courses, requires less CPU for compiling, and is easier to configure in Maven and Gradle. You can adapt the steps to your development environment using reference documentation links provided throughout the guide. 

### Sign up for Gatling Enterprise Cloud

Gatling Enterprise Cloud is a fully managed SaaS solution for load testing. Sign up for a [trial account](https://auth.gatling.io/auth/realms/gatling/protocol/openid-connect/registrations?client_id=gatling-enterprise-cloud-public&response_type=code&scope=openid&redirect_uri=https%3A%2F%2Fcloud.gatling.io%2Fr%2Fgatling) to run your first test on Gatling Enterprise Cloud. The [Gatling website](https://gatling.io/features) has a full list of Enterprise features.

### Install Gatling 

{{< alert info >}}
**Prerequisites**  
Java 11, 17, or 21 64-bit OpenJDK LTS (Long Term Support) version installed on your local machine. We recommend the [Azul JDK](https://www.azul.com/downloads/?package=jdk#zulu).
{{< /alert >}}

This guide uses the Gatling bundle, which is accessed by downloading and extracting the following `zip`file:

{{< button title="Download Gatling" >}}
https://repo1.maven.org/maven2/io/gatling/highcharts/gatling-charts-highcharts-bundle/{{< var gatlingVersion >}}/gatling-charts-highcharts-bundle-{{< var gatlingVersion >}}-bundle.zip
{{< /button >}}

## Simulation construction 

This guide introduces the basic Gatling HTTP features. Gatling provides a cloud-hosted web application
[https://computer-database.gatling.io](https://computer-database.gatling.io) for running sample simulations. You'll learn how to construct simulations
using the Java SDK. Code examples for Kotlin and the Scala SDK are available in the Documentation.

### Learn the simulation components

A Gatling simulation consists of the following:

- importing Gatling classes, 
- configuring the protocol (commonly HTTP),
- describing a scenario, 
- setting up the injection profile (virtual user profile).

The following procedure teaches you to develop the simulation from each constituent component. If you want to skip ahead
and copy the final simulation, jump to [Test execution](#test-execution). Learn more about simulations in the
[Documentation]({{< ref "/reference/script/core/simulation" >}}). 

#### Setup the file 

Once you have downloaded and extracted the Gatling `zip` file, open the project in your integrated development
environment (IDE). Gatling recommends the [IntelliJ community edition](https://www.jetbrains.com/idea/download/). 

1. Navigate to and open `user-files/simulations/computerdatabase/ComputerDatabaseSimulation.java`.
2. Modify the simulation by deleting everything below line 7 `import io.gatling.javaapi.http.*;`.
3. The simulation should now look like the following:

{{< include-code "ScriptingIntro1SampleJava.java#setup-the-file" java >}}

#### Extend the `Simulation` class 

You must extend Gatling's `Simulation` class to write a script. To extend the `Simulation` class, after the import statements, add: 

{{< include-code "ScriptingIntro1SampleJava.java#extend-the-simulation-class" java >}}

#### Define the protocol class

Inside the `ComputerDatabaseSimulation` class, add an `HTTP protocol` class. Learn about all of the
`HttpProtocolBuilder` options in the [Documentation]({{< ref "/reference/script/protocols/http/protocol" >}}). For
this example, the `baseUrl` property is hardcoded as the Gatling computer database test site, and the `acceptHeader` and
contentTypeHeader` properties are set to `application/json`.  

{{< include-code "ScriptingIntro2SampleJava.java#define-the-protocol-class" java >}}

#### Write the scenario

The next step is to describe the user journey. For a web application, this usually consists of a user arriving at the
application and then a series of interactions with the application. The following scenario mocks a user arriving on the
home page of the [Gatling sample application](https://computer-database.gatling.io).

{{< include-code "ScriptingIntro3SampleJava.java#write-the-scenario" java >}}

See the [Documentation]({{< ref "/reference/script/core/scenario" >}}) for the available scenario
components. 

#### Define the injection profile

The final component of a Gatling simulation is the injection profile. The injection profile is contained in the `setUp`
block. The following example adds 2 users per second for 60 seconds. See the
[Documentation]({{< ref "/reference/script/core/injection" >}}) for all of the injection profile options. 

{{< include-code "ScriptingIntro4SampleJava.java#define-the-injection-profile" java >}}

Congrats! You have written your first Gatling simulation. The next step is to learn how to run the simulation locally
and on Gatling Enterprise Cloud. 

## Test execution

Now, you should have a completed simulation that looks like the following: 

{{< include-code "ComputerDatabaseSimulation.java#full-example" java >}}

### Run the Simulation locally

The open-source version of Gatling allows you to run simulations locally, generating load from your computer. Running a
new or modified simulation locally is often useful to ensure it works before launching it on Gatling Enterprise Cloud.
Using the bundle, you can launch your test with the following command in the project `bin` directory:

```console
# Mac OS and Linux
./gatling.sh

# Windows
gatling.bat
```

Select `1 Run the Simulation locally` to start the test.

When the test has finished, there is an HTML link in the terminal that you can use to access the static report. 

### Run the Simulation on Gatling Enterprise Cloud

Gatling Enterprise Cloud is a feature-rich SaaS platform that is designed for teams and organizations to get the most
out of load testing. With the trial account you created in the
[Sign up for Gatling Enterprise Cloud](#sign-up-for-gatling-enterprise-cloud) section, you can upload and run your test
with advanced configuration, reporting, and collaboration features. 

#### Generate a package

Gatling Enterprise deploys packages containing your compiled Simulations and resources. The packages must be generated
and uploaded to Gatling Enterprise. Using the bundle, the following procedure will generate your package:

1. Run `./gatling.sh` (Mac OS/Linux) or `gatling.bat` (Windows) in the `bin` directory.
2. Select option 3 `Package the Simulation for Gatling Enterprise` by typing `3` and pressing enter.
3. Note the path of the `.jar file`.

#### Create a Simulation on Gatling Enterprise Cloud

To run your Simulation on Gatling Enterprise Cloud you must upload the package and create a Simulation. The following
procedure:

1. Log in to your Gatling Enterprise Cloud account.
2. **Click** _Create a new simulation_.
  {{< img src="create.png" alt="create.png" >}}
3. **Click** _Create a simulation with a package_.
  {{< img src="simulationWithPackage.png" alt="simulationWithPackage.png" >}}
4. Fill out the _Name_ and select a _Team_.
5. **Click** _Create a new package_.
  {{< img src="createPackage.png" alt="createPackage.png" >}}
6. Enter a package name in the _Name_ field.
7. **Click** _Browse files_ in the Create Package modale and select the `.jar` file from the [Generate a package](#generate-a-package) section.
8. **Click** _Save_.
  {{< img src="generatePackage.png" alt="generatePackage.png" >}}
9. Select the Class name from the dropdown menu.
10. Select Paris under the _Locations configuration_.
11. **Click** _Save and launch_ at the bottom of the page.

Watch the Simulation deploy automatically and generate real-time reports.  

## Keep learning

You have successfully run your first test! To keep learning, we recommend the following resources:

 - [Gatling Academy](https://gatling.io/academy) 
 - [Introduction to the Recorder]({{< ref "recorder" >}})
 - [Writing realistic tests]({{< ref "advanced" >}})
