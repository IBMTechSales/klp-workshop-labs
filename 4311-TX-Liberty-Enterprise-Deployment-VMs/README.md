# Lab 4311: 

## Recommended practices for WebSphere Liberty deployments and zero-migration upgrades on VMs

The goal of this lab is to provide hands-on experience using recommended
practices for deploying Java applications to Liberty in collectives on
VMs, using automation and flexible deployment methodologies.

You will learn how Liberty dynamic routing feature enables routing of
HTTP requests to all members of Liberty collectives without regenerating
the WebSphere plug-in configuration file when servers, collective
members, applications, or virtual hosts are added, removed, started,
stopped, or modified.

You will also learn that Liberty follows a single stream continuous
delivery model. There is only one stream of Liberty, no version upgrades
necessary. Simply install new version (or fixpacks if you prefer) to get
the latest performance enhancements, features, and bug fixes. Then use
your existing build process to produce a new Liberty Server package that
contains the updated Liberty binaries, your existing application and
configuration, and then deploy to a dual installation location. This
removes one of the largest headaches for managing technical debt for
your applications: keeping current on software updates.

Following these methodologies, you will gain an understanding of how you
might apply your own DevOps processes or automation to achieve
significant agility and flexibility managing Liberty deployments with
repeatable automated processes that significantly reduces risk to your
business.

After completing the lab, you should have an appreciation for how simple
Liberty is to manage though automation, which equally applies to
integration with your own DevOps tools.

**This lab contains the following hands-on activities:**

  - Build Liberty server packages using Liberty's flexible deployment model

  - Create a Liberty-ND Collective 

  - Deploy Liberty Server packages to the Liberty-ND collective

  - Configure Liberty-ND Dynamic Routing in the Collective Controller

  - Upgrade Liberty using Liberty's zero-migration architecture


## The lab environment

The lab environemnt consists of two Host VMs:

  - server0.gym.lan

  - server1.gym.lan

You will use Linux shell scripts provided for the lab, to build Liberty
server packages, construct a Liberty- ND Collective that spans two host
VMs, and deploy the server package to both host VMs.

The Liberty-ND Collective that you will create is illustrated below:

![](./images/media/image1.png)

The “**server0.gym.lan**” host VM, which is the primary VM, contains the
following components:

  - **Liberty Builds and Server packages:** Think of this as a “build
    machine” where a build process builds the applications, runs tests,
    and produces a server package that is ready to be deployed. A server
    package contains the Liberty binaries, application, default server
    configurations, which get deployed as a unit to host VMs.

  - **Collective Controller:** The collective controller is a Liberty
    server that is configured with the “**collectiveController-1.0**”
    feature, which enables the Server to act as the management server
    for the collective.

> **Note:** In most cases the Collective Controller would likely be
> placed on a dedicated host, but to minimize the size of this
> demonstration environment, it’s collocated with the host used for
> builds

  - **Collective Member:** Collective members are Liberty Servers that
    run your application and are joined to the collective with the
    “**collectiveMember-1.0**” feature. Collective members can be
    centrally managed and take advantage of features such as “dynamic
    Routing” without requiring Liberty ND licenses for the collective
    members.

> **Note:** In most cases Liberty collective members are not located on
> the same host as collective controllers, but to minimize the size of
> this demonstration environment a collective member is collocated with
> a collective controller.

  - **Http Server:** The IBM HTTP server is used in some labs to
    showcase Liberty capabilities such as Dynamic Routing, Session
    Persistence, and fail-over scenarios.

**Note:** In most cases the HTTP server is placed on a dedicated host
located in the DMZ, but to minimize the size of this demonstration
environment, it’s collocated with Liberty processes.

The “**server1.gym.lan**” VM contains the following components:

  - **Collective Member:** Collective members are Liberty Servers that
    run your application and are joined to the collective with the
    “**collectiveMember-1.0**” feature.

## Accessing the environment

1.  Access the lab environment from your web browser.
    
    The lab environment page is displayed, the lab environment contains
 two (2) Linux VMs, **server0** and **server1**. The VM **server0** is
 the one with the Graphical User Interface (GUI) for you to access and
 work in this lab.
 
    ![Screens screenshot of a computer Description automatically
 generated](./images/media/image2.png)

2.  Click **`server0`** icon to access it.

    <img src="./images/media/image3.png" width="200"/>
   
    
3.  If prompted in the VM login screen, login to the **server0** VM
    using the credentials below:

    User ID: **`techzone`**
 
    Password: **`IBMDem0s!`**
 
    **Note:** That is a numeric zero in **IBMDem0s!**

4.  The `server0`` VM GUI is displayed, click the RedHat icon in the middle
    of screen to bring up the VM desktop UI.

    ![A screenshot of a computer Description automatically
 generated](./images/media/image4.png)
 
    The VM desktop UI is displayed.
 
    ![A screenshot of a computer Description automatically
 generated](./images/media/image5.png)


**Tips for working in the lab environment:**

1.  You can use the VM Desktop tools to fit to window or resize the
    window.

    ![A screenshot of a computer Description automatically
 generated](./images/media/image6.png)

2.  You can copy / paste text from the lab guide into the lab
    environment using the VM Desktop Send Text tool.

    ![A computer screen shot of a keyboard Description automatically
 generated](./images/media/image7.png)
 
    a. Copy the text from the lab guide that you want to paste into the
 lab environment.
 
    b. Open a terminal window, or a text editor or a web browser in the VM
 Desktop where you want to copy the text to.
 
    c. Click the **`Send Text`** icon in the VM Desktop tool bar, paste the
 text into its window, then again click **`Send Text`** to send the text
 to an open command shell and close its window.
 
    ![A screenshot of a computer Description automatically
 generated](./images/media/image8.png)
 
    The text is now copied to the terminal window, or the text editor or
 the web browser you opened in the previous step.
 
    ![A screenshot of a computer Description automatically
 generated](./images/media/image9.png)

## Review Liberty deployment common practices

A Liberty server is lightweight due to its modular architecture, so you
can easily package a server installation and applications in a
compressed “zip” or “jar” package. You can then store this package and
use it to deploy the installation to different nodes or machines in your
Liberty Collective.

In this lab, you will deploy Liberty and sample applications to a
Liberty-ND Collective, while following several common practices as
illustrated below.

![](./images/media/image10.png)

  - **Recommended practice:** Produce server packages as build
    output

    It is recommended to create an immutable build using server packages
 that include the Liberty binaries, server configuration, application,
 and shared configuration as build output.
 
    The build output, “**server package**”, is the deployable unit to
 Liberty collective members. Using this practice is very similar to
 recommended practices for container image deployments in Kubernetes
 platforms.

  - **Recommended practice:** Automate the build and deployment of
    server packages to the collective

    Automating installation, deployment, and configuration is always
 recommended to achieve greater agility, repeatability, and
 productivity.

  - **Recommended practice:** Add configuration overrides to the
    server after the server package is uncompressed.

    The automation scripts used in the lab follows this practice. The
 server package is built as a template that contains the application,
 libraries, and default configuration.
 
    Then, when the server package is deployed and uncompressed on the
 target machine, the configuration overrides are added. These overrides
 can override any default configuration from the server package.
 

<table>
<tbody>
<tr class="odd">
<td><img src="./images/media/image11.png" style="width:2.82431in;height:0.82431in" alt="sign-info" /></td>
<td><p><strong>Note:</strong> there are several alternatives to applying the overrides to the server after expanding the archive.</p>
<p>Some clients choose to override using OS environment variables that override default Values in the server.xml, other clients apply the overrides in the Liberty configuration by building the archive using the overrides in either the configDropins directory or via an include(d) external xml file in the server.xml for a specific environment.</p></td>
</tr>
</tbody>
</table>



## Part 1: Clone the GitHub repo for this workshop

This lab requires artifacts that are stored in a GitHub repository. Run
the command below to clone the repository to the local VM used for the
lab.

1.  Open a new terminal window on the “**server0.gym.lan**” VM.

    <img src="./images/media/image12.png" width="200"/>


2.  Copy the commands below to the terminal window to clone the GitHub
    repository required for the lab.

        git clone https://github.com/IBMTechSales/liberty_admin_pot.git
 
        cd /home/techzone/liberty_admin_pot/lab-scripts
 
        chmod -R 755 ./
 
    ![A screenshot of a computer Description automatically
 generated](./images/media/image13.png)

  


## Part 2: Produce Liberty “server packages” as build output

Following recommended practices for flexible deployment of Liberty
applications, you will produce a server package as build output, which
includes the Liberty runtime, server configuration, and the application,
as a zip archive file.

Producing the build output in the form of a Liberty server package zip
file provides the flexibility of deploying and upgrading your version of
Liberty and applications as an immutable package, like how container
images are deployed to Kubernetes container platforms.

In the lab, the **`mavenBuid.sh`** script provides the following
capabilities for producing server packages for deployment to a Liberty
collective.

  - Pull the application source code from the source code repository
    (GitHub)

  - Build the application and produce a Liberty Server package

  - Store the Liberty server package in a “working directory” for the
    lab.

<table>
<tbody>
<tr class="odd">
<td><img src="./images/media/image11.png" style="width:1.82431in;height:0.82431in" alt="sign-info" /></td>
<td><p><strong>mavenBuild.sh script is NOT an official IBM tool.</strong></p>
<p>It is a simple script that we provide for this PoT to demonstrate ease of automation of common Liberty tasks. Other tools such as Gradle, Jeknins, UCD, etc can be employed based on enterprise preferences and practices</p></td>
</tr>
</tbody>
</table>

In this section of the lab, you will use the provided shell script that
automates the tasks for producing a server package for deployment to the
collective.

**Use the Maven Build script to produce a Server package**

1.  Run the **Maven Build** shell script to build the applications and
    produce a Liberty Server package, which will use WebSphere Liberty
    kernel, version 22.0.0.8

        /home/techzone/liberty_admin_pot/lab-scripts/mavenBuild.sh -v 22.0.0.8
 
    **Note:** there are additional steps performed aside from what is
 depicted in the output above which only shows the completion.
 
    ![A screenshot of a computer Description automatically
 generated](./images/media/image14.png)

2.  Using the File viewer on the VM desktop, see that the server package
    was produced.

    a. Double mouse-click on the “**Home”** folder on the Desktop VM.
 
    <img src="./images/media/image15.png" width="200"/>

    b. From the file explorer, navigate
 to **techzone/lab-work/packagedServers** directory.
 
    **TIP:** the server package is named based on the version of Liberty
 in the package, and the placeholder server name;
 “**22.0.0.8-pbwServerX.zip**".
 
    ![A screenshot of a computer Description automatically
 generated](./images/media/image16.png)


**What did the Maven Build do?**

The core activity performed by the script is to run Maven to build the
applications and produce a Liberty Server package. The server package is
somewhat customized to include additional artifacts and configuration
overrides that are required to run the applications in Liberty.

The Maven build process leverages the “**Liberty Maven Plugin**”, which
provides for the capabilities to retrieve Liberty binaries from the
maven repo, build the application, and create a Liberty server package.

As illustrated below, Maven configures Liberty using the artifacts
provided in the projects and produced by the build.

  - Maven adds the server.xml file and the application binaries (WAR,
    EAR)

  - Maven adds the configDropins/overrides as required for the
    environment:

> ![](./images/media/image17.png)

**Below is a hi-level list of tasks the maven build process performs in
this lab:**

  - Download the Liberty Kernel based on the version specified on the
    command; for example, version 22.0.0.8

  - Build the application deployable artifacts for **PlantsByWebSphere**
    and **WhereAmI** applications: EAR, WAR, JAR

  - Create a Liberty Server named “**pbwServerX**” as a template server
    that is used for multiple deployments to the collective.

  - Add the two example applications to the server configuration

  - Add the required DB2 libraries to the server

  - Replace the server.xml server configuration file with the server.xml
    generated by Transformation Advisor

  - Add the “**memberOverrides.xml**” config/overrides file

  - Install all the Liberty features as required by
    the **server.xml** file

  - Install the Collective Member feature so servers can be included as
    members in a Liberty Collective

  - Install the Session Database feature so the application will
    function with session persistence with fail-over

  - Produce the Liberty Server package as a zip file, containing the
    Liberty binaries, applications, and the default configurations

The output from the “**mavenBuild**” script is a Liberty Server package.
The server package is in the following working directory.

    /home/techzone/lab-work/packagedServers

  <br/>

**Congratulations!** You have used Maven and successfully produced a
Liberty server package, which adheres to the flexible deployment
recommended practices.

<br/>

Now that you have a server package, it can be deployed to local or
remote hosts (VMs / machines) where the Liberty collective members will
host the sample applications.

In the next sections of the lab, you will continue the recommended
practice of using automation to create a `Liberty Collective`` and deploy
the server package to two hosts (VMs), and add the deployed servers to
the Liberty Collective, where the servers can be centrally managed by
the collective.


## Part 3: Create a Liberty Collective Controller

A **`Liberty-ND Collective`** is a set of Liberty servers in a single
management domain.

A collective consists of at least one server with
the **collectiveController-1.0** feature enabled that is called
a ***collective controller***.

**TIP:** Liberty Servers that function as Collective Controllers MUST
have Liberty ND licenses, as these servers use
the **collectiveController-1.0** feature that is only available with
Liberty-ND.

<table>
<tbody>
<tr class="odd">
<td><img src="./images/media/image11.png" style="width:2.12431in;height:0.82431in" alt="sign-info" /></td>
<td><p>The collective controller provides for a centralized administrative control point to perform operations such as MBean routing, file transfer, operational control, and monitoring</p>
<p>A core role of collective controllers is to receive information, such as MBean attributes and operational state, from the members within the collective so that the data can be retrieved readily without having to invoke an operation on each individual member.</p></td>
</tr>
</tbody>
</table>

A collective can have many servers with
the **`collectiveMember-1.0`** feature enabled in application servers
that are called ***collective members.***

![](./images/media/image18.png)

In this section of the lab, you will create the **`Collective`** and
the **`Collective Controller`** using automation, via
the **`createController.sh`** shell script.

The “**createController.sh**” script provides the following capabilities

  - Create the Collective and Collective Controller

  - Install the **Liberty Admin Center** application into the Controller
    server

  - Start the Collective Controller server


1.  Run commands below in the same command shell as you used to build
    the serverPackage, to create a Liberty collective controller:

        /home/techzone/liberty\_admin\_pot/lab-scripts/createController.sh
 
    The createController.sh script creates a Liberty server
 named **CollectiveController.**

    The CollectiveController server is in the following directory:

    > /home/techzone/lab-work/liberty-controller/wlp/usr/servers

    - The CollectiveController server is configured with
    the **`collectiveController-1.0`** feature which enables the server
    to act as the managing server for a collective

    - The CollectiveController server is also configured with
    the **`adminCenter-1.0`** feature, which installs the “Liberty Admin
    Center” UI application.

    - The CollectiveController server runs on HTTPS port **9491** in this
    lab

    ![](./images/media/image19.png)

2.  Once the collective controller is started, click its **Admin Center
    URL** to launch it in a browser window, then enter the login
    credentials as: **admin** / **admin**.

    **Note:** If you see the “Warning: Potential Security Risk Ahead”, \>
 click **Advanced..-\>scroll down and \_\>Accept Risk and Continue** to
 continue.
 
    ![A screenshot of a computer Description automatically
 generated](./images/media/image20.png)

3.  Login to the **Admin Center** using
    credentials: **admin** / **admin**.

    ![](./images/media/image21.png)
 
    The Liberty Collective “**Admin Center”** UI is displayed.
 
    ![](./images/media/image22.png)

4.  Click the **`Explore`** icon to display the servers, applications, and
    hosts in the Collective.

    ![Icon Description automatically
 generated](./images/media/image23.png)
 
    The collective resource list is displayed, and you can see that you
 have:

    - one server – The collective controller server

    - one host – the local host that the controller is running on

    - one runtime – Liberty runtime

    ![Graphical user interface, application Description automatically
 generated](./images/media/image24.png)



## Part 4: Create Liberty Collective Members

The collective members are the Liberty servers that run your
applications. For Liberty servers to join a collective, the servers must
have the **`collectiveMember-1.0`** feature enabled.

Membership in a Liberty collective is optional. Liberty servers join a
collective by registering with a collective controller to become
members. Members share information about themselves with the controller
through the controller operational repository.

![](./images/media/image25.png)

In this section of the lab, you will join Liberty servers as collective
members to the collective, using the server package that you produced
previously in the lab.

That server package you created, includes the Liberty binaries, the
sample applications, and default server configuration overrides.

The **collectiveMember-1.0** feature was installed and enabled for the
Liberty server that is in the server package.

In this lab, you use the “**`addMember.sh`**” script to deploy the server
packages to the nodes, create the collective members, and join the
members to the collective.

**The addMember.sh script performs the following tasks:**

  - Register the Host machine if it is remote VM from the Controller

  - Copy or send the server package to the host machine where Liberty
    will be deployed

  - Unzip the server package, which is an archive installation of
    Liberty on the hosts machines (VMs)

  - Apply server configuration overrides for the specific collective
    member

  - Join the collective member to the collective

  - Open application port and Collective Controller port for remote
    hosts

This script adds collective members, one to the local host VM,
server0.gym.lan, another to the remote host VM, server1.gym.lan.

Use the automation script to deploy the Liberty servers from the server
package you created earlier and join them as a member to the collective.

1.  In the same command shell as before, run the addMember.sh script
    twice with different input parameters as shown to create two Liberty
    servers.

        /home/techzone/liberty_admin_pot/lab-scripts/addMember.sh -n  appServer1 -v 22.0.0.8 -p 9081:9441 -h server0.gym.lan
 
       
        /home/techzone/liberty_admin_pot/lab-scripts/addMember.sh -n  appServer2 -v 22.0.0.8 -p 9082:9442 -h server1.gym.lan
 
 When the script completes, the server **appServer1** and
 server **appServer2** are created and added to the collective.

2.  Go back to the Liberty collective **Admin Center** page and you can
    see the total number of servers is now 3 with the **appServer1**
    and **appServer2** added.

    ![A screenshot of a computer error Description automatically generated
 with low confidence](./images/media/image26.png)

3.  Click the **SERVERS** icon to go to its details page.

    ![A picture containing text, font, number, line Description
 automatically generated](./images/media/image27.png)
 
    You see the **appServer1** and **appServer2** have been added to the
 server list and they are in the **Stopped** state.
 
    ![A screenshot of a computer Description automatically
 generated](./images/media/image28.png)



## Part 5: Verify the application deployment in the collective

You have deployed two Liberty servers as the collective members. In this
section, you will start these two servers from the Liberty Admin Center
and run the example applications on the individual Liberty servers to
ensure the applications run properly.

1. Start the DB2 application database for PlantsByWebSphere

    The PlantsByWebSphere application requires an application database,
which you need to ensure is up and running.

    a.  Before starting the Liberty servers, you need to start the db2
    database used by the **PlantsByWebSphere** application with the
    command below.

        docker start db2_demo_data

2. Start the Liberty servers from the Admin Center

    a.  To start the collective member from the Liberty Admin
    Center **Explorer** page, click the **SERVERS** icon to go to its
    details page.

    ![A picture containing text, font, number, line Description
 automatically generated](./images/media/image27.png)

    b.  In the server details page, click the dropdown menu icon
    of **`appServer1`** and select **`Start`** to start the server.

    ![Graphical user interface, application Description automatically
 generated](./images/media/image29.png)
 
    **Note:** If prompted for credentials, enter the Admin Center username
 and password as: **admin** / **admin**.

    c.  Click **`Start`** to confirm the start **appServer1** server command.

    ![A screenshot of a computer Description automatically generated with
 medium confidence](./images/media/image30.png)
 
    Server **appServer1** will start, and you can see it is now in
 the **Running** state.
 
    The appServer1 server now shows it has two applications `running`, which
 are used in the labs in this workshop.

    - PlantsByWebSphere

    - WhoAmI

    ![A screenshot of a computer Description automatically
 generated](./images/media/image31.png)

3.  Repeat the same server start procedure for **`appServer2`** server.
    Once it is done, the **appServer2** server is started as show below:

    ![A screenshot of a computer Description automatically generated with
 medium confidence](./images/media/image32.png)

4.  Click the **`Explorer`** dashboard icon to go back to the dashboard
    view.

    ![Graphical user interface, application Description automatically
 generated](./images/media/image33.png)
 
    Explore the “**Applications**”, “**servers**”, and “**hosts**” that
 are registered and running in the collective
 
    ![A screenshot of a computer Description automatically generated with
 medium confidence](./images/media/image34.png)
 
    **Applications view:**
 
    ![A screenshot of a computer Description automatically
 generated](./images/media/image35.png)
 
    **Servers view:**
 
    ![A screenshot of a computer Description automatically
 generated](./images/media/image36.png)
 
    **Hosts view:**
 
    ![A screenshot of a computer Description automatically generated with
 medium confidence](./images/media/image37.png)

### 5.1 - Test the two example applications used in the lab

In this section, you will test the two applications that are deployed in
the collective.

**Test the PlantsByWebSphere application:**

1.  To access the **PlantsByWebSphere** application on **appServer1**

    a. Open a new tab on the Firefox browser, enter the following URL to
 test **PlantsByWebSphere** on **appServer1**, which is
 on **server0.gym.lan**
 
        https://server0.gym.lan:9441/PlantsByWebSphere
 
    **Note:** If you see the “Warning: Potential Security Risk Ahead”, \>
 click **Advanced..-\>scroll down and -\>Accept Risk and Continue** to
 continue.
 
    ![A screenshot of a website Description automatically generated with
 low confidence](./images/media/image38.png)
 
    b. In the application, click on the “**Flowers**” tab to view the
 catalog of flowers. This action retrieves catalog details from the
 application DB2 database.
 
    ![A screenshot of a computer Description automatically generated with
 medium confidence](./images/media/image39.png)

2.  Repeat the steps to access the **PlantsByWebSphere** application on
    **appServer2** on host **server1.gym.lan**

    a. Open a new tab on the Firefox browser and test
 **PlantsByWebSphere** on **appServer2**, which is
 on **server1.gym.lan**
 
        https://server1.gym.lan:9442/PlantsByWebSphere
 
    **Note:** If you see the “Warning: Potential Security Risk Ahead”, \>
 click **Advanced..-\>scroll down and -\> Accept Risk and Continue** to
 continue.
 
    ![A screenshot of a website Description automatically generated with
 low confidence](./images/media/image40.png)

**Test the WhereAmI application:**

1.  To access the **WhereAmI** application on **appServer1.**

    a. Open a new tab on the Firefox browser and enter the following URL
 to test **WhereAmI** on **appServer1**, which is
 on **server0.gym.lan**
 
        https://server0.gym.lan:9441/WhereAmI
 
    ![A screenshot of a computer Description automatically
 generated](./images/media/image41.png)

2.  Repeat the steps to access the **WhereAmI** application on
    **appServer2** on host **server1.gym.lan.**

    a. Open a new tab on the Firefox browser and test **WhereAmI**
 on **appServer2**, which is on **server1.gym.lan**
 
        https://server1.gym.lan:9442/WhereAmI
 
    ![A screenshot of a computer Description automatically
 generated](./images/media/image42.png)

3.  **Close** the browser windows / tabs displaying the
    **PlantsByWebSphere** and **WhereAmI** applications.



## Part 6: Configure Dynamic Routing application High Availabilty

In this section, you configure the `Dynamic Routing`` feature to route HTTP
requests to members of Liberty collectives without having to regenerate
the WebSphere plug-in configuration file when the environment changes.

The Dynamic Routing feature, **`dynamicRouting-1.0`**, provides the
Dynamic Routing service, which dynamically retrieves routing information
from the collective repository and delivers this information to the
WebSphere plug-in.

To configure dynamic routing for a Liberty collective, you need to
perform the following tasks:

  - **Add dynamicRouting-1.0 feature to the Collective controller**

    This feature must be added to the Collective Controller’s server.xml
 file.

  - **Create a Plug-in configuration file for the HTTP Server**

    The “dynamicRouting setup” command generates the “keystore” and
 “plug-in configuration files” required for dynamic routing.

  - **Establish a secure connection between the plug-in and the
    collective controller**

   The generated plug-in configuration file and keys must be copied to
 the appropriate locations to establish the secure connection.



### 6.1 - Setup Dynamic Routing in WebSphere Liberty

In this section, you will use an automation script, which we provide in
the lab environment, to perform the steps described above to setup and
configure Dynamic Routing.

1.  Run the `setupDynamicRouting.sh` script shown below in the same
    command shell you used previously, to setup the plugin configuration
    for dynamic routing.

    The **setupDynamicRouting.sh** script performs all the tasks described
 above which configures dynamic routing in the collective
 
        /home/techzone/liberty_admin_pot/lab-scripts/setupDynamicRouting.sh
 
    Once the command is completed, the pug-in configuration files are
 created and configured for the IHS server.
 
    ![Text Description automatically
 generated](./images/media/image43.png)

**Dynamic routing in the Liberty Collective is now ready to use\!**

### 6.2 - Examine the generated “plugin-cfg.xml” file

The **`plugin-cfg.xml`** file contains configuration information that
determines how the web server plug-in forwards requests to the Liberty
servers in the collective.

The plugin only needs to connect to the Collective Controller to get
topology information. It does not need to know the host/port of the
application servers.

The plugin-cfg.xml file is in the following directory:

>  **/opt/IBM/WebSphere/Plugins/config/webserver1**

1.  Examine the generated **plugin-cfg.xml**

        gedit /opt/IBM/WebSphere/Plugins/config/webserver1/plugin-cfg.xml
 
    With Dynamic Routing, HTTP requests are sent to members of Liberty
 collectives without regenerating the WebSphere plug-in configuration
 file when the environment changes.
 
    **Note:** The plugin-cfg.xml no does not contain the host and port
 information for the application servers or the application URL, etc.
 as is the case with the static HTTP server plugin. Instead, the
 plugin-cfg.xml contains the host and port information for the
 collective controller which provides the application and application
 server information dynamically to the plugin.
 
    When servers, cluster members, applications, or virtual hosts are
 added, removed, started, stopped, or modified; the new information is
 dynamically delivered to the WebSphere plug-in from the Liberty
 Collective Controller.
 
    In this configuration, requests are routed based on up-to-date
 information.
 
    ![A screenshot of a computer program Description automatically
 generated with low confidence](./images/media/image44.png)

2.  **Close** the **gedit** editor. DO NOT SAVE ANY CHANGES\!

### 6.3 - Examine the Web Server’s “httpd.conf” file

The **`httpd.conf`** file contains the HTTP Server configuration.

The WebSphere plug-in module is loaded by appending configuration to the
httpd.conf file in the web server.

The web server’s httpd.conf file is in the following directory:

> **/opt/IBM/HTTPServer/conf**

1.  Examine the generated **httpd.conf** file

        gedit /opt/IBM/HTTPServer/conf/httpd.conf
 
    a. Scroll to the last line of the httpd.conf file, which is the
 configuration to load the WebSphere plugin module.
 
    b. Notice the configuration points to the **plugin-cfg.xml** file,
 which is used to determine how to direct the http requests to the
 Liberty servers in the collective.
 
    ![A picture containing text, screenshot, line, font Description
 automatically generated](./images/media/image45.png)

2.  **Close** the **gedit** editor. **DO NOT SAVE ANY CHANGES**\!




## Part 7: Testing the Dynamic Routing Features

In this section, you are going to test the dynamic routing you
configured for the Liberty collective.

You are going to conduct two testing scenarios:

  - In the first test case, you are using
    the **PlantsByWebSphere** application to test the high
    availability of the application and verify you can always access the
    application directly from the IHS server if at least one of the
    member servers is running.

    When you stop one of the app servers, the dynamic routing
 automatically redirects the traffic to another surviving app server
 without any user intervention or application interruption.

  - The second test case demonstrates round robin load balancing and the
    dynamic routing distributes traffic to the collective members based
    on their workloads.

    The **WhereAmI** application is used in this testing because it does
 NOT use sticky sessions, whereas the PlantsByWebSphere application
 does.
 
    When you refresh the application URL link in the web browser window,
 you can see the dynamic routing performs round-robin style routing
 among the servers.

**Test Case 1:**

This test case uses PlantsByWebSphere application. The design of this
application uses HTTP Sessions to store application state in the
internal http session object. By default, the http session object is
local to the Liberty server, and not persisted in any external store.

WebSphere traditional and WebSphere Liberty use a JSESSIONID in this
case, which identifies the server handing the request that includes uses
an http session. Then on subsequent transactions or requests, the
JSESSIONID is read by the web server plugin, and requests continue to be
routed to the SAME server.

If the server handing the requests goes down, then the web server plugin
will redirect the requests to any surviving servers.

However, without session persistence configured, any session data is
lost, such as items in a shopping cart, or login cookies, etc.

1.  To access the **PlantsByWebSphere** application through **IHS**
    server and plugin, open a new browser window and enter the
    application URL as:

        https//server0.gym.lan:8443/PlantsByWebSphere
 
    The application “**Home”** page is displayed.
 
    ![Graphical user interface, website Description automatically
 generated](./images/media/image46.png)

2.  You can navigate and visit different pages of the application. You
    can see that although the application is running on two Liberty
    servers with different HTTP/HTTPS ports, the dynamic routing
    function of the Liberty collective is able to direct the incoming
    traffic through the specified IHS server port (8080) to the
    application.

    ![A screenshot of a computer Description automatically generated with
 medium confidence](./images/media/image47.png)

3.  Click the **Help** link to go to application **Help** page.

    ![A green and white rectangle with white text Description
 automatically generated with low
 confidence](./images/media/image48.png)
 
    The application Help page is displayed. On this page, you can see
 which Liberty server the request was routed.
 
    As showing in the screen shot below, the application is running
 from **appServer2** which might be different in your case.
 
    ![A screenshot of a computer Description automatically generated with
 medium confidence](./images/media/image49.png)

4.  **Stop** the Liberty server that is identified as handling the
    request, as shown on the PlantsByWebSphere
    application **"Help"** page.

    a. Go back to Liberty collective **Admin Center Servers** page.
 
    b. **Stop** the server that was identified on the application Help
 page, as illustrated below:
 
    ![Graphical user interface, application Description automatically
 generated](./images/media/image50.png)
 
    If prompted, enter the Admin Center credentials as: **admin / admin**.
 
    The server is stopped.
 
    ![Graphical user interface, application, website Description
 automatically generated](./images/media/image51.png)

5.  From the **PlantsByWebSphere** application page, click the
    “**Flowers**” tab, to show the catalog of flowers.

    ![A picture containing text, screenshot, font, line Description
 automatically generated](./images/media/image52.png)

6.  Click the **Help** link to go to application **Help** page, you can
    see now the application is running from a different application
    server.

    ![A screenshot of a computer Description automatically generated with
 medium confidence](./images/media/image53.png)
 
    This demonstrates that the Liberty dynamic routing detects the
 application server is down and directs the traffic to another
 application server automatically.
 
    **The application high availability test is completed.**

**Test Case 2:**

In this test case, the **WhereAmI** application is used. This
application does not use http sessions, and therefore the web server
plugin can direct requests to the Liberty servers in a round-robin
style.

1.  **`Start`** both application servers (**`appServer1`** and
    **`appServer2`**) from Liberty collective Admin Center.

2.  Open a new browser window and enter the **`WhereAmI`** application URL
    as:

        https://server0.gym.lan:8443/WhereAmI
 
    The output shows that currently the application is running
 from **appServer1** server.
 
    In your test, you may see **appSever2** handing the initial request.
 
    ![Graphical user interface, text, application, email Description
 automatically generated](./images/media/image54.png)

3.  Refresh the application page by clicking the **refresh icon** on the
    browser.

    You can see the output showing that Liberty dynamic routing feature
 directs the request traffic to other application server in a
 round-robin fashion.
 
    ![A screenshot of a computer Description automatically
 generated](./images/media/image55.png)

4.  Refresh the browser again a few more times and see that the requests
    get routed to **appServer1** and **appServer2** accordingly.


    **The round robin load balancing test is completed.**



## Part 8: Produce a new “server package” using Liberty 22.0.0.12

In this section of the lab, you will use the same automated
**`mavenBuild`** script that was used in previous labs to produce a
NEW Liberty server package, with only one notable difference; the server
package will include Liberty `22.0.0.12` instead of 22.0.0.8.

Producing the build output in the form of a Liberty server package zip
file provides the flexibility of deploying and upgrading your version of
Liberty and applications as an immutable package, like how container
images are deployed to Kubernetes container platforms.

In this section of the lab, you will use the provided shell script that
automates the tasks for producing a server package for deployment to the
collective.

**Use the Maven Build script to produce a Server package with Liberty
22.0.0.12**

1.  Run the following command to build the applications and produce a
    server package, which will use WebSphere Liberty kernel, version
    22.0.0.12

        /home/techzone/liberty_admin_pot/lab-scripts/mavenBuild.sh -v 22.0.0.12
 
    Take note that a new Liberty server package was created with the name:
 **`22.0.0.12-pbwServerX.zip`**
 
    ![A screenshot of a computer Description automatically
 generated](./images/media/image56.png)
 
    The output from the “**mavenBuild**” script is a Liberty Server
 package.
 
    The server package is in the following working directory.
 
    > **/home/techzone/lab-work/packagedServers**

2.  Using the File viewer on the VM desktop, see that the server package
    was produced.

    a. Double mouse-click on the “**Home”** folder on the Desktop VM
 
    <img src="./images/media/image15.png" width="150"/>

    
    b. From the file explorer, navigate
 to **`/home/techzone/lab-work/packagedServers`** directory.
 
    **TIP:** the server package is named based on the version of Liberty
 in the package, and the placeholder server name;
 “**22.0.0.12-pbwServerX.zip**.
 
    ![A screenshot of a computer Description automatically
 generated](./images/media/image57.png)

   **Congratulations\!** You have used Maven and successfully produced a
new Liberty server package that contains your applications and WebSphere
Liberty 22.0.0.12.

<br/>

   In the next sections of the lab, you will continue the best practice of
using automation to deploy the server package to two hosts (VMs) and
join the deployed servers to the Liberty Collective.



## Part 9: Deploy the new server package to the Collective

In this section of the lab, you deploy new Liberty servers as collective
members to the collective, using the server package that you produced in
the previous section of the lab.

That server package you created, includes the Liberty binaries for
22.0.0.12 and your SAME sample applications using the SAME default
server configuration as the previous deployment in Liberty 22.0.0.8.

In this lab, you use the same **`addMember.sh`** script used in previous
labs to deploy the server packages to the nodes, deploy the server
package, create the collective members, and join the members to the
collective.

**Launch the Liberty Admin Center in the Web Browser**

1.  If the Liberty Adin Center is not already open in the Web Browser,
    open it now, using the following URL:

        https://server0.gym.lan:9491/adminCenter/

2.  Login to the **Admin Center** using
    credentials: **admin** / **admin**.

    ![](./images/media/image21.png)
 
    The Liberty Collective “**Admin Center”** UI is displayed.
 
    ![](./images/media/image22.png)

3.  Click the **`Explore`** icon to display the servers, applications, and
    in the Collective.

    ![Icon Description automatically
 generated](./images/media/image58.png)

4.  Click the **`Servers`** view to display the servers in the Collective.

    **Note:** You should already see two servers deployed in the
 collective.
 
    The servers are running **Liberty 22.0.0.8**. And the server should be
 in the “**Running**” state.

5.  **If the appServer1 or appServer2 servers are NOT running, go ahead
    and start them now.**

    These servers were deployed in the previous labs.
 
    ![](./images/media/image59.png)

### 9.1 - Deploy collective member to the local host VM, server0.gym.lan

Use the automation script to deploy the Liberty server from the server
package you created earlier and join the member to the collective.

**The script below performs the following:**

  - deploy the server package for Liberty version 22.0.0.12

  - Apply overrides for the HTTP and HTTPS ports with “9081 / 9441”

  - Rename the default server name with “appServer1”

  - Join the server to the collective


1.  From the same terminal window on the VM, run the command below to
    create a local Liberty collective member on **server0.gym.lan** VM
    using the 22.0.0.12 server package.

        /home/techzone/liberty_admin_pot/lab-scripts/addMember.sh -n appServer1 -v 22.0.0.12 -p 9081:9441 -h server0.gym.lan
 
    ![A screenshot of a computer Description automatically
 generated](./images/media/image60.png)
 
    When the script completes, the server **appServer1** with Liberty
 22.0.0.12 is created and added to the collective.
 
    The **addMember.sh** script created a local Liberty server
 called **appServer1** in the following directory on
 the **server0.gym.lan** VM:
 
    > **/home/techzone/lab-work/liberty-staging/22.0.0.12-appServer1/wlp/usr/servers**

2.  Go back to the Liberty collective Admin Center’s Server page. You
    can see that the **`appServer1`** server in directory
    **`22.0.0.12-appServer1`** is added to the collective as a new
    server.

    The Server is in the “**Stopped**” state.
 
    ![](./images/media/image61.png)

### 9.2 - Deploy a collective member to the remote host VM, server1.gym.lan

Now, run the script again, using slightly different parameters, to
deploy Liberty to the remote VM, **server1.gym.lan**. And, then join the
remote member to the collective.

Joining remote members to a collective requires a couple of additional
steps that the script performs for you in the lab environment and are
identified below.

1.  Run command below to create a remote Liberty collective member
    on **server1.gym.lan** VM, specifying a different server name and
    ports.

        /home/techzone/liberty_admin_pot/lab-scripts/addMember.sh -n  appServer2 -v 22.0.0.12 -p 9082:9442 -h server1.gym.lan
 
    ![A screenshot of a computer Description automatically
 generated](./images/media/image62.png)

  - When the script completes, the server **`appServer2`** with Liberty
    `22.0.0.12` is created and added to the collective.

  - The **addMember.sh** script created a local Liberty server
    called **appServer2** in the following directory on
    the **server1.gym.lan** VM:

    > **/opt/IBM/liberty-staging/22.0.0.12-appServer2/wlp/usr/servers**

  - The server uses **9082** and **9442** as its HTTP/HTTPS ports, as
    defined ad script input parameters.


2.  Go back to the Liberty collective Admin Center’s “**servers**” view. You can see the new member, **`appServer2`**, has been added to
    the server list and is the **`Stopped`** state.

    ![A screenshot of a computer Description automatically generated with
 medium confidence](./images/media/image63.png)

### 9.3 - Ripple Start the new 22.0.0.12 servers

Now that the new Liberty servers with Liberty version 22.0.0.12 have
been deployed to the collective, all you need to do now is ripple start
the servers.

Ripple starting the servers will allow the new application servers to
accept incoming request without incurring an application outage.

The ripple start will be manually performed in the Admin Center in this
lab. However, like the other automated scripts you have used, this
process can also be easily automated.

**Steps to ripple start the new servers:**

  - Stop 22.0.0.8-appServer1

  - Start 22.0.0.12-appServer1

  - Stop 22.0.0.8-appServer2

  - Start 22.0.0.12-appServer2


1.  Before starting the Liberty servers, you need to ensure the db2
    database used by the **PlantsByWebSphere** application is running.

        docker start db2_demo_data

3.  **`Stop`** the collective member **`22.0.0.8-appServer1`** from the
    Liberty Admin Center.

    a. In the **server** details page, click the dropdown menu icon of
 “**22.0.0.8-appServer1”** and select **Stop** to stop the server.
 
    ![](./images/media/image64.png)
 
    **Note:** If prompted for credentials, enter the Admin Center username
 and password as: **admin / admin**.
 
    b. Click **`Stop`** to confirm the stop
 for **22.0.0.8-appServer1** server
 
    ![A screenshot of a computer error Description automatically generated
 with medium confidence](./images/media/image65.png)
 
    Server **appServer1** will stop, and you can see it is now in
 the **Stopped** state.
 
    ![A screenshot of a computer Description automatically generated with
 medium confidence](./images/media/image66.png).

4.  **Start** the collective member “**22.0.0.12-appServer1**” from the
    Liberty Admin Center.

    a. In the **server** details page, click the dropdown menu icon of
 “**22.0.0.12-appServer1”** and select **Start** to start the server.
 
    ![](./images/media/image67.png)
 
    b. Click **`Start`** to confirm the stop
 for **22.0.0.12-appServer1** server.
 
    ![A screenshot of a computer Description automatically generated with
 medium confidence](./images/media/image68.png)
 
    Server **22.0.0.12-appServer1** will start, and you can see it is now
 in the **Running** state.
 
    ![A screenshot of a computer Description automatically generated with
 low confidence](./images/media/image69.png).

**Checkpoint of the current state**

At this point, you should see the following server states:

**On VM server0.gym.lan:**

  - 22.0.0.8-appServer1 - **Stopped**

  - 22.0.0.12-appServer1 -**Running**

    ![A screenshot of a computer Description automatically generated with
medium confidence](./images/media/image70.png)

**On VM server1.gym.lan:**

  - 22.0.0.8-appServer2 - **Running**

  - 22.0.0.12-appServer2 -**Stopped**

    ![A screenshot of a computer Description automatically generated with
medium confidence](./images/media/image71.png)

Next, ripple start the **22.00.12-appServer2** server on
VM **server1.gym.lan** following the same steps as above.

1.  **`Stop`** the collective member **`22.0.0.8-appServer02`** from the
    Liberty Admin Center.

5.  **`Start`** the collective member **`22.0.0.12-appServer02`** from the
    Liberty Admin Center.

    The final state should reflect the 22.0.0.12 servers are RUNNING, and
the 22.0.0.8 servers are STOPPED.

    ![](./images/media/image72.png)

    **Congratulations\!** You have just completed the upgrade from Liberty
22.0.0.8 to 22.0.0.12 using WebSphere Liberty’s zero-migration
architecture and common practices for deployments using immutable server
packages for flexible deployments.

    </br>

    The final activity in this lab is to demonstrate the applications
continue to run as-is after the upgrade.



## Part 10: Test the applications after the Liberty upgrade

You have successfully ripple started the new 22.0.0.12 servers in the
collective.

In this section, you will test the PlantsByWebSphere and WhereAmI
applications and ensure the applications run properly after the upgrade
to Liberty 22.0.0.12.


### 10.1 - Test the PlantsByWebSphere application:

1.  To access the **PlantsByWebSphere** application on appServer1

    a. Open a new tab on the Firefox browser and test PlantsByWebSphere
 on **appServer1**, which is on **server0.gym.lan**
 
        https://server0.gym.lan:9441/PlantsByWebSphere
 
    **Note:** You will likely see the “Warning: Potential Security Risk
 Ahead”, click Advanced-\>. scroll down-\>Accept Risk and Continue to
 continue. The warning occurs because new self-signed certs were
 created when deploying the new application server version.
 
    ![](./images/media/image73.png)
 
    b. In the application, click on the “**Help**” link, located on the
 upper right corner of the application page.
 
    ![](./images/media/image74.png)
 
    c. On the “**Help**” page, you will see the Server name and the
 version of Liberty that is running for the server that handled this
 specific request.
 
    ![A screenshot of a computer Description automatically generated with
 medium confidence](./images/media/image75.png)
 
    d. In the application, click the “**Home**” link to return to the
 PlantsByWebSphere Home page.
 
    ![](./images/media/image76.png)

2.  OPTIONAL: Repeat the steps to access
    the **PlantsByWebSphere** application on appServer2 on host
    server1.gym.lan

         https://server1.gym.lan:9442/PlantsByWebSphere
 
    **Note:** If you see the “Warning: Potential Security Risk Ahead”,
 click Advanced..-\>Accept Risk and Continue to continue.
 
    ![](./images/media/image77.png)

### 10.2 - Test the WhereAmI application:

1.  To access the **WhereAmI** application on appServer1

    a. Open a new tab on the Firefox browser and test WhereAmI
 on **appServer1**, which is on **server0.gym.lan**
 
        https://server0.gym.lan:9441/WhereAmI
 
    b. Note that the application is running on Liberty version 22.0.0.12
 
    ![](./images/media/image78.png)

2.  OPTIONAL: Repeat the steps to access the **WhereAmI** application on
    appServer2 on host server1.gym.lan

        https://server1.gym.lan:9442/WhereAmI
 
    ![](./images/media/image79.png)

### 10.3 - Test Dynamic Routing after the Liberty upgrade

In this part of the lab, you demonstrate that Liberty’s Dynamic Routing
capabilities is now automatically directing incoming requests from the
HTTP server to the new Liberty 22.0.0.12 servers, after the upgrade.


1.  To access the **WhereAmI** application through the IBM HTTP Server
    and plugin, open a new browser window and enter the application URL
    as:

        https://server0.gym.lan:8443/WhereAmI

    The output shows the application running on **appServer1** on the
 initial request.
 
    **Note:** It is possible that the request is routed
 to **appServer2** instead of appServer1.
 
    Importantly, note the Liberty Version is: **22.0.0.12**, validating
 that the Dynamic Routing capability has automatically detected the new
 Liberty Servers and is directing incoming requests to the new Liberty
 22.0.0.12 servers after the upgrade.
 
    ![](./images/media/image80.png)

2.  Refresh the application page by clicking the
    Browsers **refresh** icon on the page.

    You can see the output showing that Liberty dynamic routing feature
 directs the request traffic to **appServer2** server.
 
    ![](./images/media/image81.png)

3.  Refresh the browser again a few more times and see that the requests
    get routed to **appServer1** and **appServer2** accordingly.

    The round robin load balancing test is completed.

**Congratulations\!**

**You have successfully completed the lab “Liberty Deployment on VMs”**


## Summary

In the lab, you followed the best practices to deploy and configure
Liberty Collective and upgrade the version of Liberty in the Collective

Following this methodology, you gained an understanding of how you might
apply your own build processes or automation to achieve significant
agility and flexibility managing Liberty collectives with repeatable
automated processes that significantly reduces risk to your business.

You have gained an appreciation for Liberty’s “**zero-migration**”
architecture and how simple it is to upgrade Liberty following the
common practices described in the lab.

**In this lab, you have completed the following administrative
activities in Liberty Collective on VMs implementation :**

  - Build Liberty server packages

  - Create a Liberty Collective

  - Deploy Liberty Server packages to the collective

  - Configure and work with Liberty Dynamic Routing fro application HA

  - Upgrade vrsion of Libery using Liberty's zero-migration architecture capabilitoes

