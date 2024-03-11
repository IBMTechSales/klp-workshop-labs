# Lab 2161-1: Rapidly deploy and configure your Java applications with WebSphere Liberty and OpenShift Container Platform

![banner](./images/media/image1.jpeg) 

**Last updated:** March 2024

**Duration:** 60 minutes

Need support? Contact **Kevin Postreich, Yi Tang**

## Overview
This lab provides fundamental hands-on experience with modernizing existing Java applications to WebSphere Liberty, deployed into a container platform, such as Red Hat OpenShift. 

The focus of this lab is on the practical aspects of application deployment and configurations and not the analysis of the Transformation Advisor results. *Other labs cover Transformation Advisor in detail.*

Upon completion of this lab, you will have gained skills to download and use the `Transformation Advisor migration bundle` to deploy and configure your application in the following scenarios: 

1.  **to a locally running WebSphere Liberty** - useful for deploying to Liberty in a VM

2.  **to a container image** – useful for deploying to Kubernetes

3.  **using the Liberty Operator** – useful for deploying to OpenShift


**IBM Cloud Transformation Advisor** (Transformation Advisor) is an application modernization tool that is entitled through IBM Cloud Pak for Applications and WebSphere Hybrid Edition. Transformation Advisor helps you quickly evaluate on-premises Java EE applications for deployment to the cloud. 

The Transformation Advisor tool provides the following value:

  - Identifies the Java EE programming models in the app

  - Determines the complexity of replatforming these apps by listing a high-level inventory of the content and structure of each app

  - Highlights Java EE programming model and WebSphere API differences between the WebSphere runtime profile types 

  - Identifies Java EE specification implementation differences that might affect the app

  - Generates accelerators for deploying the application to Liberty and containers in a target environment

Additionally, the tool provides a recommendation for the right-fit IBM WebSphere Application Server edition and offers advice, best practices, and potential solutions to assess the ease of moving apps to Liberty or newer versions of WebSphere traditional. It accelerates application migrating to cloud process, minimizes errors and risks and reduces time to market.


## Objective

The objectives of this lab are to:

  - Learn how to download the application migration bundle and use it to deploy an application to WebSphere Liberty running locally

  - Learn how to fully populate the migration bundle placeholders and build the application into a container image

  - Learn the role of Kustomize when deploying the migration bundle

  - Learn how to deploy your application to OpenShift with a single command

  - Learn how to create multiple configurations for the application and deploy them to OpenShift


## Prerequisites

The following prerequisites must be completed prior to beginning this lab:

  - Familiarity with basic Linux commands

  - Have internet access

  - Access to the TechZone lab environment

## Accessing the lab environment

If you are doing this lab as part of an instructor led workshop (virtual or face to face), an environment has already been provisioned for you. The instructor will provide the details for accessing the lab environment.

Otherwise, you will need to reserve an environment for the lab. You can obtain one here. Follow the on-screen instructions for the “**Reserve now**” option.

<https://TBD-to-the-reservation-link>

The lab environment contains six (6) Linux VMs.

![A screenshot of a computer error Description automatically generated](./images/media/image2.png)

1.  Access the lab environment from your web browser.

    A `Published Service` is configured to provide access to the **`Workstation`** VM through the noVNC interface for the lab environment.
 
    a. When the demo environment is provisioned, click on the **`environment tile`** to open its details view.
 
    b. Click on the **`Published Service`** link which will display a **`Directory listing`**
 
    c. Click on the **`vnc.html`** link to open the lab environment through the **`noVNC`** interface.
 
    ![A list of directory listing Description automatically generated](./images/media/image3.png)
 
    d. Click the **`Connect`** button
 
    ![A screenshot of a computer Description automatically generated](./images/media/image4.png)
 
    e. Enter the password as: **`passw0rd`**. Then click the **`Send Credentials`** button to access the lab environment.
 
    Note: That is a numeric zero in passw0rd
 
    ![A screenshot of a login box Description automatically generated](./images/media/image5.png)

2.  If prompted to Login to the "workstation" VM, use the credentials below:

> The login credentials for the **workstation”** VM is:

  - User ID: **techzone**

  - Password: **IBMDem0s\!**

> Note: That is a numeric zero in the password
> 
> ![student vm screen](./images/media/image6.png)

## Tips for working in the lab environment

1. You can resize the viewable area using the **noVNC Settings** options to resize the virtual desktop to fit your screen.

    a. From the environment VM, click on the **`twisty`** on the noNC control pane to open the menu.

    ![fit to window](./images/media/image7.png)

    b. To increase the visible area, click on **`Settings > Scaling Mode`** and set the value to **`Remote Resizing`**

    ![fit to window](./images/media/image8.png)

2. You can copy / paste text from the lab guide into the lab environment using the clipboard in the noVNC viewer.

    a. Copy the text from the lab guide that you want to paste into the lab environment

    b. Click the **`Clipboard`** icon and **`paste`** the text into the noVNC clipboard

    ![fit to window](./images/media/image9.png)

    c. Paste the text into the VM, such as to a terminal window, browser window, etc.

    d. Click on the **`clipboard`** icon again to close the clipboard

    An alternative to using the noVNC Copy / Paste option, you may consider opening the lab guide in a web browser inside of the VM. Using this method, you can easily copy / paste text from the lab guide without having to use the noVNC clipboard.

## Lab Tasks

In this lab, you will use the Transformation Advisor `migration bundle` to build a container image and deploy an application to a local container for testing.

To simplify the lab and to allow you to focus on the migration bundle, certain software and artifacts have already been put in place for you. These are as follows:

  - **Transformation Advisor** has been installed and collected data has been loaded

  - **docker** (for creating and running images) has been installed

  - **oc** (OpenShift command Line tool for running OCP commands) has been installed

  - The **PlantsByWebSphere Sample Application**, built as an Enterprise Archive file (EAR) is available from the lab resource provided.

  
# Part 1: Application Assesment    

## 1.1 Pull down the Lab artifacts and setup lab environment

1.  If you have not yet cloned the GitHub repo with the lab artifacts, in a previous lab, run the following command on your terminal:

    a.  Open a new Terminal window and clone the git repository to pull down the lab artifacts

    ![A screen shot of a computer Description automatically generated](./images/media/image10.png)

    b.  Run the commands to clone the git repository to the local system

        cd /home/techzone

        git clone https://github.com/IBMTechSales/appmod-pot-labfiles.git

    c.  **Add “execute” permissions to the shell scripts**

        find ./appmod-pot-labfiles -name "\*.sh" -exec chmod +x {} \\;

2.  Run the provided shell script to setup the lab environment.

    The lab-setup.sh script moves files from the cloned git repo to a **student** directory used in the lab.
 
        cd /home/techzone/appmod-pot-labfiles/labs/RuntimeModernization/scripts
 
        ./lab-setup.sh
 
    When complete, you will see the following output
 
     
    ==========================
 
     lab-setup script completed
 
    ==========================
    

## 1.2 Launch Transformation Advisor (local)

Transformation Advisor provides a **`migration plan`** for each application being assessed for modernization. The migration plan includes a **`migration bundle`** of generated artifacts that accelerate the deployment of applications to Liberty in containers and Kubernetes / OpenShift.

The migration bundle includes diverse artifacts, depending on the needs of the application to accelerate the build and deployment of an application Container image into an OpenShift container platform.

The Transformation Advisor is installed locally on the **Workstation** VM. 

1. Launch the Transformation Advisor tool using the steps below.

    a.  From **Workstation** VM Desktop Tool Bar, click the `Terminal` icon to open a Terminal window.

    ![](./images/media/image11.png)

    b.  Launch the **Transformation Advisor** with commands:

        cd /home/techzone/transformation-advisor-local-3.8.1

        ./launchTransformationAdvisor.sh

     Wait for Transformation Advisor to initialize and display the **action menu** list.

    c.  Type **`5`** and press **`Enter`** to “Start” the **Transformation Advisor**.

     ![](./images/media/image12.png)
 
    d. The **Transformation Advisor** application is started, **`right-click`** the application URL link and select **`Open Link`** to launch it in a web browser window.
 
     The URL is displayed in the output from the TA command: [**http://server0.gym.lan:3000**](http://server0.gym.lan:3000)
 
     ![](./images/media/image13.png)
 
     The **Transformation Advisor** Home page is displayed in the Web Browser.
 
     ![](./images/media/image14.png)

    In the next section, you will create a new “**Workspace**” in Transformation Advisor and upload the saved results from the scan of a WebSphere Application Server that has a single application deployed, named “PlantsByWebSphere”.


## 1.3 Create new workspace and upload scan results from a WebSphere Application Server

In this section, create a new workspace named **'proof_of_concept'**.

A `workspace` is a designated area that will house the migration recommendations provided by Transformation Advisor based on the Data Collector scan results of your application server environment.

1. Create a new workspace named **proof-of-concept**

    a.  From the Transformation Advisor Home page, click the **`Create New`** button

    b.  Type **`proof_of_concept`** as the Workspace name. Then click the **`Create`** button

     ![A screenshot of a computer Description automatically generated](./images/media/image15.png)

2. Upload an existing **'scan results'** file provided for the lab.

    The scan results file is provided for you.

    It was produced by running the Transformation Advisor **Data Collector** on a WebSphere Application Server that had the **PlantsByWebSphere** application deployed.

    a. Click the **`Upload`** button

    b. Click the **`Drop or add file`** link. Then click the **`Upload`** button

    ![A screen shot of a computer Description automatically generated](./images/media/image16.png)

    c.  Navigate to **Home \> Techzone \> appmod-pot-labfiles \> labs \> RuntimeModernization**

    d.  Select **`pbw-collection.zip`** file. Then click the **`Open`** button

    ![A screenshot of a computer Description automatically generated](./images/media/image17.png)

    e. Once the **pbw-collection.zip** has been added, click on the **`Upload`** button to upload the results into Transformation Advisor.

    ![A screenshot of a computer Description automatically generated](./images/media/image18.png)

    After a few moments, the application data will be uploaded to the Transformation Advisor UI.

    The "**proof_of_concept**" workspace displays the “**All Java Applications**” page which shows the recommendations for the workspace.

    There is a single application called **plantsbywebsphereee6.ear**. 
    
    By default, "WebSphere Liberty" is selected as a modernization target. All the information provided assumes that this application will be modernized to WebSphere Liberty.

    ![A screenshot of a computer Description automatically generated](./images/media/image19.png)


## 1.4 Analyze the plantsbywebsphereee6 application

 Transformation Advisor provides detailed information about each application that has been analyzed. This application has a complexity of **`Simple`**.
 
 **Simple** means:
 
  - The application is ready to be deployed to WebSphere Liberty
  - No changes to the source code are required
 
   
![A screenshot of a computer Description automatically generated](./images/media/image20.png)

 ***How is this application ready for deployment when it shows 4 issues?***
 
 In this case, the application has four **`Informational`** issues.
 
 Informational issues do not prevent the application from executing on the new runtime (WebSphere Liberty) but there may be small changes in the application’s behavior.
 
 If unexpected behavior is found during testing, then reviewing these **Informational** issues may help explain what is happening.

1.  We are now ready to review the **'migration plan'** for this application. 

    a. Click on the **`Migration plan`** link at the end of the row of the **plantsbywebsphereee6.ear** application.

    ![A screenshot of a computer Description automatically generated](./images/media/image21.png)

    b.  The **Migration Plan** page opens.

    This page contains a summary of the application being migrated:

    - a preview of the files that will help during deployment (highlighted below)

    - a list of the application’s dependencies.

    All the files can be downloaded in a single convenient **migration bundle**.

    ![](./images/media/image22.png)

2. At the bottom of the screen, there is a `Application Dependencies` section. 
    
    This shows all the files, in addition to the application, that are required for a deployment. **Plantsbywebspheeee6.ear** has two dependencies.

    a. Expand the section it to see the details

    b. In this case, DB2 drivers called **db2jcc.jar** and **db2cc_licence.jar** are required for deployment

    ![A screenshot of a computer Description automatically generated](./images/media/image23.png)

3.  Download the migration bundle by clicking the **`Download`** button in the bottom right. 

    A zip file named “**plantsbywebsphereee6.ear_migrationBundle.zip**” will download to your **Download** folder.

    ![A screenshot of a computer Description automatically generated](./images/media/image24.png)


We will go through a step-by-step process of using the Transformation Advisor migration bundle to deploy **plantsbywebsphereee6.ear** as follows:

  1. Ensure the application can run locally on WebSphere Liberty

  2. Build an immutable container image running on WebSphere Liberty

  3. Deploy the image to OpenShift (and configure it) using a single command

  4. Redeploy and reconfigure this image for a new environment with a single command

# Part 2: Deploy PlantsByWebSphere app to a locally running WebSphere Liberty    

In this step, you will create a new Liberty server to run the PlantsByWebSphere application.

Then, you will review the migration bundle to see what files you need to add, and use the provided `server.xml` Liberty configuration file. This file is used to configure Liberty by providing values for ports, security, context routes and by providing application-specific configuration. 

### 2.1   Create a local Liberty Server

1.  From a **`Terminal`** window, create a new local WebSphere Liberty server named ‘**pbwserver**”

        /home/techzone/wlp/bin/server create pbwserver

    **output:**

    ```
    Server pbwserver created.
    ```

2.  From the **Terminal** window, start the local WebSphere Liberty server.

        /home/techzone/wlp/bin/server start pbwserver

    **Output:**

    ```
    Starting server pbwserver.

    Server pbwserver started with process ID #####
    ```

3.  Confirm the local WebSphere Liberty server is running. Open the `Firefox` web browser, open a new browser tab. Then go to: **http://localhost:9080**

    ![A screenshot of a computer Description automatically generated](./images/media/image25.png)


    

### 2.1.3 Review the placeholder files in the migration bundle

In this section, you will review the migration bundle to see what files need to be added to the WebSphere Liberty configuration to run the application. 

1.  Let’s make note of the 3 `placeholder files` that are placed in the `migration bundle`.

    The placeholder files are convenient references to let you know which files you will need to copy to the **Liberty server**.
 
    *Note: You will fully populate the migration bundle in later steps*.

    a.  From the **Terminal window**, list the placeholder files in the migration bundle.

        unzip -l /home/techzone/Downloads/plantsbywebsphereee6.ear\_migrationBundle.zip | grep placeholder
 
    ![](./images/media/placeholder-files.png)
 
    - The **`target`** directory contains the placeholder file for the PlantsByWebSphere application EAR deployment file. This is a reminder that you need to copy the PlantsByWebSphere EAR file the Liberty server.

    - The **`src/main/liberty/lib`** directory contains the placeholder files for the DB2 database libraries required by the application. This is a reminder that you need to copy the DB2 libraries to the Liberty server.

    <br/>

### 2.1.4   Configure the Liberty server for PlantsByWebSphere

In this section, extract the migration bundle that you downloaded. Them. copy the required dependency files and server configuration to the Liberty server.

  - Copy the **DB2 libraries** to the directory defined in the "DB2 driver configuration" in the server.xml file.

  - Copy the **PlantsByWebSphere EAR** file to the “apps” directory of the Liberty server

  - Copy the **server.xml** file from the migration bundle to the Liberty server, replacing the default server.xml file


1. Extract the migration bundle that you downloaded

        mkdir /home/techzone/Student/labs/appmod/migration-bundle

        cd /home/techzone/Student/labs/appmod/migration-bundle

        cp /home/techzone/Downloads/plantsbywebsphereee6.ear_migrationBundle.zip .

        unzip plantsbywebsphereee6.ear_migrationBundle.zip

2.	Copy the PlantsByWebSPhere application binary file into the `apps` location in Liberty

        cp /home/techzone/appmod-pot-labfiles/labs/RuntimeModernization/plantsbywebsphereee6.ear /home/techzone/wlp/usr/servers/pbwserver/apps

3. Create the `global shared` directory in Liberty. 

    This is where you will copy the DB2 libraries for Liberty

        mkdir /home/techzone/wlp/usr/shared/config/lib/global
 
4.	Copy the DB2 driver files into the `global shared` location in Liberty.

    This will make it available to any application running in Liberty.

        cp /home/techzone/Student/LabFiles/db2_drivers/* /home/techzone/wlp/usr/shared/config/lib/global


5.	Copy the `server.xml` file from the migration bundle into the Liberty server to replace its existing configuration. Choose to overwrite the existing file if prompted.

        cp /home/techzone/Student/labs/appmod/migration-bundle/src/main/liberty/config/server.xml /home/techzone/wlp/usr/servers/pbwserver 

6.  Run the PlantsByWebSphere application from the browser.

        http://localhost:9080/PlantsByWebSphere
 
    Now the application is running in Liberty, and the main page is displayed.
 
    ![A website page of a garden Description automatically generated](./images/media/image36.png)

7.  Attempt to click on any of the tabs: “**Flowers**”, **Fruits & Vegetables**”, or “**Trees**”.

    ___

    **Note the exception.** This is an expected erorr. The issue is in the JPA persistence (Database access). A user is not defined, and therefore authentication to the database failed.
 
    ![A screen shot of a computer Description automatically generated](./images/media/image37.png)
 
    ___

    These pages should display a catalog of items in their respective category, which are rereived from the application database. 

    **So what happened?** 
    
    Transformation Advisor does not collect any `sensitive data` for the application server.  This means the application-specific configuration information in the **server.xml** file has not been set. In this case, the `username` and `password` to access the database are missing.

    In the next section, you will review the server.xml file and add the sensitive data required to access the application database.  

    <br/>

### 2.1.5   Review the server.xml file

Now you will review the **`server.xml`** file and set the necessary configuration information. 

The **server.xml** file defines a set of **`features`** that the application requires. By importing only the necessary **features** to support the application’s API needs, the footprint of the deployed application and Liberty server is kept as small as possible.

1. Review the server.xml file

    Using the “**`gedit`**” editor in a **`Terminal`** window, open the **server.xml** file located in the Liberty server

        gedit /home/techzone/wlp/usr/servers/pbwserver/server.xml

    - **Section 1** contains the `features` that the application requires, and these are discovered automatically during analysis performed the Transformation Advisor **Data Collector**.

      ![A screenshot of a computer Description automatically generated](./images/media/image38.png)

    - **Section 2:** Defines the `resources` required to access the database

      - The **`authdata`** defies the BD2 user and password that is used by the datasources. These refer to variables that are defined in the server.xml file.

      - The **`jdbc`** driver defines the required libraries. These are the libraries you copied into this location via the script

      ![A computer code with green and white text Description automatically generated](./images/media/image39.png)

    - **Section 3:** The **`datasource`** contains all the information required to access the database: Database user, password, database name, host, and port number.

      ![A computer screen shot of a program Description automatically generated](./images/media/image40.png)


    - **Section 4** contains the variables for the **`non-sensitive configuration data`**. For example, the port to run the server on. The values of these variables are collected by Transformation Advisor.

      ![A screen shot of a computer code Description automatically generated](./images/media/image41.png)

    - **Section 5** contains the **`sensitive data variables`**. 
    
      You can see that the values for these variables are all **blank**, as this information is **never** collected by Transformation Advisor.

      ![A screenshot of a computer program Description automatically generated](./images/media/image42.png)


2.  The reason that the **PlantsByWebSphere** application retuned an “Exception” is because the values for the **sensitive variables** has not been set. 
  
    ![](./images/media/image43.png)

    Next, you will update the server.xml to include the credentials required to access the application database. 


### 2.1.6   Update the server.xml and retest the PlantsByWebSphere application


1. From a Terminal window, `stop` the Liberty server named ‘pbwserver”

        /home/techzone/wlp/bin/server stop pbwserver

2. From the `gedit` editor that you have open, update the values in the **server.xml** file for the sensitive data as illustrtated below:

   a. Scroll to the bottom of the server.xml file to view the sensitive-data variables. 

   b.  Set the default value for **rhel9_baseNode01_pbwuser_password** to: **`db2inst1-pwd`**
    
   c. Set the default value for **rhel9_baseNode01_pbwuser_user** to: **`db2inst1`**

     ![](./images/media/image44.png)
    
   d. **`Save`** and **`close`** the server.xml in the editor


3. From a Terminal window, `start` the Liberty server named ‘pbwserver”

        /home/techzone/wlp/bin/server start pbwserver


4. Reload and test PlantsByWebSphere application in the browser

        http://localhost:9080/PlantsByWebSphere


    a. Click the “**Flowers**” tab. The catalog of flowers should now be displayed.

    ![A screenshot of a computer Description automatically generated](./images/media/image45.png)


3. From a **Terminal** window, `Stop` the Liberty server named ‘**pbwserver**”

        /home/techzone/wlp/bin/server stop pbwserver


### Checkpoint

At this point in the lab, you have successfully demonstrated the PlantsByWebSphere application runs successfully in WebSPhere Lberty on a local VM. 

In the next sections, you will deploy the PlantsByWebSphere application to containers and Red Hat OpenShift. 

# Part 3: Create and run the PlantsByWebSphere application as a container image    


## 3.1  Explore the Containerfile used to build PlantsByWebSphere as a container image

In the previous section, you used the **server.xml** to get the  application running on a local Liberty instance. This was to show you how the Transformation Advisor **server.xml** file is used. If you are moving to Liberty in VMs as your new runtime, then you are done! 

However. if containers are to be your final destination, this section explores the Transformation Advisor migration bundle artifacts that accelerate application deployment to Lberty in containers. 

In the case of **`Simple`** applications, it is not necessary to carry out a separate step of deploying to a local Liberty instance at all. Instead, by using the Transformation Advisor migration bundle, you can deploy your application to Liberty running in a container all in one go. 

This is what we will do now.


1.	Add the PlantsByWebSphere binary file to your migration bundle, and remove the placeholder.

        cp /home/techzone/appmod-pot-labfiles/labs/RuntimeModernization/plantsbywebsphereee6.ear /home/techzone/Student/labs/appmod/migration-bundle/target  

        rm /home/techzone/Student/labs/appmod/migration-bundle/target/plantsbywebsphereee6.ear.placeholder


2.	Add the db2 library files to your migration bundle, and remove the placeholder.

        cp /home/techzone/Student/LabFiles/db2_drivers/* /home/techzone/Student/labs/appmod/migration-bundle/src/main/liberty/lib  

        rm /home/techzone/Student/labs/appmod/migration-bundle/src/main/liberty/lib/*.placeholder

3.	Update the server.xml file to remove the TLS configuration which is not used. 

        cp /home/techzone/appmod-pot-labfiles/labs/RuntimeModernization/deploy-config-updates/apps-pbw/server.xml /home/techzone/Student/labs/appmod/migration-bundle/src/main/liberty/config 


4.  The `migration bundle` is ready to be used to generate an image of your application running on WebSphere Liberty. To do this, we will use the **`Containerfile`** that comes with the migration bundle.

    ![A screenshot of a computer Description automatically generated](./images/media/image49a.png)

    a. Explore the Containerfile in the migration bundle

        gedit /home/techzone/Student/labs/appmod/migration-bundle/Containerfile
 
 
    - The **FROM** statements in the **Containerfile** pull in the following two images. 
     
      - **`Open JDK 8`**
      - **Latest `WebSphere Liberty`**

      ![](./images/media/image50.png)

      ![](./images/media/image51a.png)

       
    - The **RUN** commands in the Containerfile create the necessary folder structures and copies the **`binary files`** from the migration bundle into the appropriate locations in the image.

      ![A close-up of a computer code Description automatically generated](./images/media/image52.png)

    - There are several lines in the file that have been commented out. 
    
      By default, the **Containerfile** assumes that your application is available as a **binary** file. However, it can also be used to build your application from **source** code. The full details of how to do this can be found in **README.md** in the migration bundle.

      ![](./images/media/image53.png)

    b. `Close` the "gedit" editor. **DO NOT SAVE ANY CHNAGES TO THE FILE!** 

    <br/>


### 3.2 Setup, build and run PlantsByWebSphere in a local container

In this section, you will build and run the PlantsByWebSphere application in a local container.

1.	Build the container image from the migration bundle using docker

        cd /home/techzone/Student/labs/appmod/migration-bundle

        docker build -f /home/techzone/Student/labs/appmod/migration-bundle/Containerfile --tag apps/pbw .

    - `-f` is the path to the `Containerfile` used to build the image
    - `--tag` allows you to define the name and version of the image to build
    - `.` is the "current directory" of the migration bunlde that includes the artifacts for building the container image. 

    **Note:** Ignore the following warning message. This will be fixed in the next release of Transfromation Advisor 

    ```
    1 warning found (use --debug to expand):
    - Empty continuation line found in: RUN mkdir -p /config/apps &&     mkdir -p /sharedlibs &&     cp ./src/main/liberty/config/* /config &&     cp ./target/*.*ar /config/apps/ &&     if [ ! -z "$(ls ./src/main/liberty/lib 2>/dev/null)" ]; then         cp -r ./src/main/liberty/lib/* /sharedlibs;     fi
    ```

4. Confirm the container image has been built

        docker images | grep apps/pbw


    ![](./images/media/pbw-image-listed.png)


5.	The next step is to run the image using the `docker run`command.

        docker run -d --rm -p 9080:9080 --name pbw-app  apps/pbw

    -  `-d` start the container in 'detached' mode so you get your command prompt back
    -  `--rm` remove the container when it is stopped
    -  `-p` expose the HTTP port
    - `--network` connect the container to the same docker network as the DB2 container
    -  `--name` is the name we are giving the container 


6.	Check that the contaier is up and running. 

        docker ps | grep pbw-app

    ![](./images/media/pbw-app-up.png)



7.  From the browser, access the PlantsByWebSphere application that is running in the local container

        http://server0.gym.lan:9080/PlantsByWebSphere
 
     ![A screenshot of a web page Description automatically generated](./images/media/image61.png)
 
    
    Attempt to click on any of the tabs: “**Flowers**”, **Fruits & Vegetables**”, or “**Trees**”.

    ___

    **Note the exception.** This is an expected erorr. The issue is in the JPA persistence (Database access). A user is not defined, and therefore authentication to the database failed.
 
    ![](./images/media/pbw-container-db-error.png)
 
    The error is because we are using the unchanged server.xml file that came with the migration bundle. 
    
    As outlined already, Transformation Advisor does not collect any sensitive data so the application specific configuration information has not been set. 
    ___

**Considerations & recommendations:**

One option here is to update the **server.xml** file, add in the sensitive data values that Transformation Advisor did not collect and rebuild the image. 
    
However, a big part of container image value is that they are **`immutable`**. No matter where you take and deploy the image the operating system, runtime, security patch level, etc. will be the same. 
    
This gives you great **reproducibility** and gets away from the classic ‘**but it works for me!**’ issue. We lose much of this value if we bake in the configuration with the image, as we will have to produce an image for each new configuration. 
    
Instead of using the exact same image in each of your development, staging and production environments, you would be using different images. 

___
   
**It is never best practice to hard code configuration into your image.** 


In the next section we will look at how the `migration bundle` helps you manage this configuration easily across all your environments and how it will simplify deployment to your OpenShift cluster. The migration bundle uses `Kustomize` to help achieve this.
___



## 3.2 Understand how Kustomize is used in the migration bundle

**`kustomize`** is simple way to manage configuration across all your different deployments and environments without the need for templates.

By using **`overlays`**, it allows you to break out your basic configuration information (ports, names, hosts, etc.) from your sensitive data (usernames, password, etc.) that are likely to change in every deployment.

Every **kustomize** artifact is plain `YAML` and can be validated and processed in a standardized way. Plus, it makes them very human readable\! It is natively built into **kubectl** and the **OpenShift client**.


The structure of the kustomize folder is as follows.

  1.	`base` contains your expected standard deployment in a `config map` (because it is not sensitive data). It also contains the `application.cr-yaml` file that will execute the deployment and mount the necessary secrets that are created from the secret file.

  2.	`overlays` contains all your different deployment configurations. In this case, only a single deployment has been created for your `dev` systems. The `secret` file will contain all your application-specific **sensitive data** that will be created as secrets in OpenShift.

  > ![](./images/media/image64a.png)
 
___

**Note:** In the next section we will use this kustomize structure to deploy your application image.

Further information on **kustomize** can be found at <http://kustomize.io>
    
___


## 4 Deploy to OpenShift with config

You have now created a a container image without any application specific configuration. In this section, we will **push** that image to an image repository and then **deploy** it into OpenShift, along with its configuration, with a single command.


### 4.1 Update the YAML files to add the application configuraton for deployment

Recall, the container image does not have the application configuration baked in. 

You will make the following updates to the configuration YAML files in the migration bundle for the PlantsByWebSphere application deployment to OpenShift. 

  -  Update the `overays/dev/plantsbywebsphereee6-secret.yaml` file
     -  Add the sensitive-data to the **secret** YAML file
  
  -  Update the `base/application-cr.yaml` file
     - Add the referece to the container image
     - Disable TLS, as we do not have this configured in our environment
     - Accept the Liberty license
     
     
  - Update the `base/plantsbywebsphereee6-configmap.yaml` file
     -  Add the database host imformation to access the dev database

___

**TIP:** To perform the updates, you will copy updated versions of the yaml files from the lab artifacts that we provide for the lab. This way you can focus on the outcomes, rather that the tedious task of editing text files.    
___

1.	Update the **plantsbywebsphereee6-secret.yaml** file, which contains the sensitive-data for the application. You will copy an updated version of the file that we have prepared for the lab

        cd /home/techzone/Student/labs/appmod/migration-bundle/deploy/kustomize/overlays/dev/

        cp  /home/techzone/appmod-pot-labfiles/labs/RuntimeModernization/deploy-config-updates/apps-pbw/plantsbywebsphereee6-secret.yaml  /home/techzone/Student/labs/appmod/migration-bundle/deploy/kustomize/overlays/dev 

    The update to the `plantsbywebsphereee6-secret.yaml` in the **overlays/dev** directory is now complete. The sensitive data is encoded and added to the yaml file. 

    ![](./images/media/secret-yaml-update.png)


    a. `How did we encode the values?` 
    
    We updated the values in the YAML file with the sensitive data by  **Base64 encoding** the values as illustrated below.
 
    - We encoded the following values:
   
      ___

      - user: **db2inst1**

        echo db2inst1 | base64

      The encoded value is: **ZGIyaW5zdDEK**

      ___

      - password: **db2inst1-pwd**

        echo db2inst1-pwd | base64

      The encoded value is: **ZGIyaW5zdDEtcHdkCg==**

      ___ 

2.  Update the `application-cr.yaml` file
    
    The `application-cr.yaml` is the WebSphere Liberty custom resource used to deploy the PlantsByWebSphere application.
  
        cd /home/techzone/Student/labs/appmod/migration-bundle/deploy/kustomize/base/

        cp  /home/techzone/appmod-pot-labfiles/labs/RuntimeModernization/deploy-config-updates/apps-pbw/application-cr.yaml  /home/techzone/Student/labs/appmod/migration-bundle/deploy/kustomize/base 

    The updated `application-cr.yaml` in the **base** directory is now complete. The PlantsByWebSphere application configuration is finalized.  

    ![](./images/media/application-cr-yaml-update.png)



3.	Update the **plantsbywebsphereee6-configmap.yaml** file, which contains the database name and host for the dev database

        cd /home/techzone/Student/labs/appmod/migration-bundle/deploy/kustomize/base/

        cp  /home/techzone/appmod-pot-labfiles/labs/RuntimeModernization/deploy-config-updates/apps-pbw/plantsbywebsphereee6-configmap.yaml  /home/techzone/Student/labs/appmod/migration-bundle/deploy/kustomize/base 

    The update to the `plantsbywebsphereee6-configmap.yaml` in the **base** directory is now complete. The database host for the dev database has been added to the yaml file. 

    ![](./images/media/pbw-configmap-yaml-update.png)

 ### 4.2 Deploy the application and its configuration to OpenShift

To deploy the PlantsByWebSphere application to OpenShift, you must perform the following basic steps.

  - Tag the container image, appropriately tagging for the OpenShift internal registry
 
  - Push the tagged container image to the OpenShift internal registry   

  - Use a single command to deploy and configure the application to openShift


1. Tag the container image 

    a. Go to a `Terminal` window   
        
        
    b. Tag the container image using the following docker command
    
        docker tag apps/pbw default-route-openshift-image-registry.apps.ocp.ibm.edu/apps-pbw/pbw-app:1.0


2. Verify the image was tagged

        docker images | grep pbw-app

    ![](./images/media/pbw-apps-tagged.png)


3. Login to OpenShift to run the commands to push and deploy the image 

        oc login -u ocadmin -p ibmrhocp

4. Create a new project where you will dpeloy the application 

        oc new-project apps-pbw


5. Login to the OpnShift internal registry so you can push the image 

        docker login -u $(oc whoami) -p $(oc whoami -t) default-route-openshift-image-registry.apps.ocp.ibm.edu 

6. Push the container image to OpenShift internal registry

        docker push default-route-openshift-image-registry.apps.ocp.ibm.edu/apps-pbw/pbw-app:1.0        

In previous steps, you configured the sensitive data that is needed, accepted the license, pointed to the image that is needed to pull,  and logged into OpenShift. Now, with a single command, you can deploy to OpenShift and configure the image. 

You must specify which overlay will provide the configuration (dev in this case).

7. Deploy the PlantsByWebSphere application to OpenShift    

        cd /home/techzone/Student/labs/appmod/migration-bundle/deploy/kustomize

        oc apply -n apps-pbw -k overlays/dev


8. Verify the deployment

        oc get deployment -n apps-pbw

    Wait for the `deployment` to be ready and availabe, as illustrated below:

        NAME                   READY   UP-TO-DATE   AVAILABLE  
        plantsbywebsphereee6   1/1     1            1    


9. Verify the appliation `pod` is ready.

    This may take a few moments. Just keep checkig with command below

        oc get pods -n apps-pbw

    Wait for the `pod` to be ready and availabe, as illustrated below:

        NAME                                    READY   STATUS       
        plantsbywebsphereee6-7476449666-shnx8   1/1     Running   

10. From the browser, navigate to the PlantsByWebSphere route for the `dev` deployment:

        http://plantsbywebsphereee6-dev.apps.ocp.ibm.edu/PlantsByWebSphere
 
    The PlantsByWebSphere main page is displayed.

    ![A website page of a garden Description automatically generated](./images/media/image78.png)

11. Click on the **`Trees`** category and view the trees that are loaded in the ‘dev’ environment database.

    This catalog of trees was retrieved from the DB2 database in the `dev` environment.
 
    ![A screenshot of a computer Description automatically generated](./images/media/image79.png)



##  4.3 Explore the WebSphere Liberty Operator in OpenShift

In this section, you will take a look at the **WebSphere Liberty Operator** in the OpenShift console to see what has been deployed.

1.  Login to OpenShift console

    a.  Open a new tab in the web browser

    b.  Click on the **`OpenShift Console`** bookmark on the bookmark toolbar

    c.  Login credentials:

    - Username: `ocadmin`

    - Password: `ibmrhocp`

2.  View the **IBM WebSphere Liberty** `Operator`

    a.  Click on **`Operators > Installed Operators`** in the left-hand menu

    b. Select `apps-pbw` in the **Project** filter

    c. Type **`Liberty`** in the **NAme** filter

    d.  Click on **`IBM WebSphere Liberty`**

    ![A screenshot of a computer Description automatically generated](./images/media/installed-operators-apps-pbw.png)


3. View the **WebSphereLibertyApplication** `Deployment`

    a.  Click on the tab called **`WebSphereLibertyApplication`**

    ![A green circle with text Description automatically generated](./images/media/image87a.png)

    b. You will see the **plantsbywebsphereee6** application listed in the ‘dev’ namespace

    b. Click on the link called **`plantsbywebsphereee6`** under the **Name** column

    ![A screenshot of a computer Description automatically generated](./images/media/image88a.png)

    c.  Select the **`Resources`** tab

    ![A screenshot of a computer Description automatically generated](./images/media/image89a.png)

    d. Select the link for the **`Deployment`**

    ![A screenshot of a computer Description automatically generated](./images/media/image90a.png)

    e. The PlantsByWebSphere deployment has one `pod` running

    ![A screenshot of a computer Description automatically generated](./images/media/image91a.png)


4.  View the WebSphereLibertyApplication **`Route`**

    a.  Return to the **plantsbywebsphereee6 operator** page

    ![A screenshot of a computer Description automatically generated](./images/media/image92a.png)

    b.  Click the **`Resources`** tab

    ![A screenshot of a computer Description automatically generated](./images/media/image92b.png)

    c.  Select the link of **Kind: `route`**

    ![A screenshot of a computer Description automatically generated](./images/media/image93a.png)

    d.  Click on the **`Location`** link for the route

    ![A screenshot of a computer Description automatically generated](./images/media/image94a.png)

    e.  The **Welcome to Liberty** page is displayed in a new Browser tab. 

    ![A screenshot of a computer Description automatically generated](./images/media/image95.png)

    f.  Append the context root ‘**PlantsByWebSphere’** to access the PlantsByWebSphere application

        http://plantsbywebsphereee6-apps-pbw.apps.ocp.ibm.edu/PlantsByWebSphere


## 4.4 Re-deploy app with different configuration to OpenShift

We have deployed the PlantsByWebSphere application with its configuration to the `dev` environemnt, using a single kustomize command. 

In this section, we will deploy the same application again with a new configuration for the `staging` environment, again using a single kustomize command.


1.	Create a new `overlay` directory called **staging** that will store the new staging configuration, coping the existing dev directory.

        cd /home/techzone/Student/labs/appmod/migration-bundle/deploy/kustomize/overlays

        cp -r dev staging

2.	Update the **plantsbywebsphereee6-configmap.yaml** file, which contains the database name and host for the **staging** environment

    To update, use the command below to copy a completed version of the config map yaml file to the new configuration for the **staging** environment.  

        cp  /home/techzone/appmod-pot-labfiles/labs/RuntimeModernization/deploy-config-updates/apps-pbw/plantsbywebsphereee6-configmap-staging.yaml  /home/techzone/Student/labs/appmod/migration-bundle/deploy/kustomize/base/plantsbywebsphereee6-configmap.yaml


    The update to the `plantsbywebsphereee6-configmap.yaml` in the **base** directory is now complete. The database **serverName** for the **staging** environment has been added to the configmap yaml file, as illustrated below. 

    ![](./images/media/staging-db-servername.png)


3.	Move back to the `kustomize` directory and delete the `dev` deployment

        cd /home/techzone/Student/labs/appmod/migration-bundle/deploy/kustomize

        oc delete -n apps-pbw -k overlays/dev

    ___
    **Note:** Typically, you would not have to delete the deployment. However, in the sample application, configuration data is cached within the app, and it is not designed to look for stale cached content. 
    ___

4. From the `kustomize` directory. apply the new `staging` overlay

        oc apply -n apps-pbw -k overlays/staging        

5. From the browser, navigate to the PlantsByWebSphere route for the application:

        http://plantsbywebsphereee6-dev.apps.ocp.ibm.edu/PlantsByWebSphere
 
    The PlantsByWebSphere main page is displayed.

    ![A website page of a garden Description automatically generated](./images/media/image78.png)


    a. Click on the `Home` link on the application main page

    ![](./images/media/home-link.png)


    b. Click on the **`Trees`** category and view the trees that are loaded in the `staging` database.

    ![](./images/media/staging-db-trees.png)

    ___
    For reference, this catalog of trees was retrieved from the `dev` database. Notice the name of the trees are different. 
 
    ![A screenshot of a computer Description automatically generated](./images/media/image79.png)

 
# Summary

___
**Congratulations!**

**You have successfully completed the core learning objectives in the lab**

___

In this lab, you learned how to deploy applications to WebSphere Liberty using the WebSphere Liberty Operator and the deployment artifacts produced by Transformation Advisor in its migration bundle.

You explored the options for deployment: 

  - locally running Liberty
  - Liberty as an image running in local container
  - Liberty as an image running in OpenShift.

You learned how to easily configure deployments to OpenShift to allow the same immutable image to be deployed for different configurations such as ‘dev’ and ‘staging’ environment deployments.

You learned about some of the practical ways you can secure your configuration data.

You explored the PlantsByWebSphere assessment details and reports in Transformation Advisor.


___

You may continue your learning journey with the optional sections of the lab. 

___


# Optional Sections: 

<details>
 <summary><b><font color="dodgerblue">Click to expand:</font></b> Section 5: (Optional) GitOps and configuration management
 </summary>

<br/>

## 5 GitOps and configuration management

At this point we have two distinct elements for deployment: the **application image** and the **configuration for that image for each potential environment**. In the GitOps model, these two elements are stored in separate Git repositories. 

You can further distribute the configuration into different repositories for each environment. This approach allows you to treat configuration as if it were code and to apply standard code development techniques such as Pull Requests, code review and maintain a full audit history. This approach delivers excellent controls and oversights for configuration changes for your OpenShift clusters, especially as you promote changes through your environments.

![A black background with white arrows and dots Description automatically generated](./images/media/image105.png)

This illustration above comes from an excellent article on real world GitOps: <https://developer.ibm.com/blogs/gitops-best-practices-for-the-real-world/>

We will not cover all of GitOps it in detail in this lab - however, there are two relevant areas that are worth mentioning briefly.

**Configuration Drift**

With the GitOps approach, there is the risk that the deployed configuration for the OpenShift cluster does not match what is in the Git repository. Someone could have updated the OpenShift cluster directly, for example. There are a number of tools to manage this configuration drift, with ArgoCD being a common tool to address this issue. It can be configured to sync manually or automatically, so that any changes made directly on the cluster are reverted to what is in the Git repository. 

The OpenShift GitOps Operator can be used to install ArgoCD into your OpenShift cluster. Further details on how to this can be found here: <https://developer.ibm.com/tutorials/deploy-open-liberty-applications-with-gitops/>

**Securing Secrets**

Even with controls and restricted access to Git repositories, it is not considered good practice to store the sensitive data in plain text (aka the values entered into the secrets files). There are generally two approaches you can take to tackle this: encrypt the values or use a reference:

  - **Encrypt the values**: In this approach, the value in the secret file is encrypted when it is committed to the Git repo. During deployment to OpenShift there is an extra step where the secret values are decrypted. `SealedSecrets` is one implementation of such an approach.

  - **Use a reference**: In this case, the secrets and their values are stored in a secret manager. During deployment, a reference is provided to the secret manager which will then mount the secret to the deployed container. `HashiCorp Vault` is a common implementation of this approach.

In both cases, there is additional work to be done above what has been covered in this lab. However, the kustomize files provided in the migration bundle gives you a good starting point to identify the secrets and provide a standard output that can be transformed to suit your selected approach.

</details>


<details>
 <summary><b><font color="dodgerblue">Click to expand:</font></b> Section 6: (Optional) Exploring the PlantsByWebSphere application in the proof_of_concept workspace
 </summary>

<br/>

## 6 Exploring the PlantsByWebSphere application in the proof_of_concept workspace

IBM Cloud Transformation Advisor (TA) analyzes application and configuration data collected from Java application server environments. TA delivers great insights that help organizations plan and implement a modernization journey to WebSphere Liberty and Kubernetes base container platform for existing Java applications.
 
TA provides recommendations for modernization which includes and considers application complexity, dependencies, potential issues, and estimated development effort.
 
![A screenshot of a computer Description automatically generated](./images/media/image106.png)

1. In a browser window, return to Transformation Advisor using the following URL: 

        http://localhost:3000

2. Open the `proof-of-concept` workspace, if it is not already open

    ![](./images/media/ta-proof-of-concept.png)


3.  Scroll down to the `Java applications` section and note the high-level summary for the ‘**plantsbywebsphereee6.ear**’application assessment.

  - Complexity

  - Issues

  - Required code changes

  - Application cost (in days for development)

    ![](./images/media/image107.png)

This workspace has only a single application name `plantsbywebsphereee6.ear`. It is summarized as `simple` to  modernize to WebSphere Liberty.  

 ***Complexity values and their meanings:***
 
**`Simple`**: The application is ready for deployment, no access to the source code is required. 

  - Simple complexity value typically represents about 20% of applications.
 
 **`Moderate`**: Code changes are required before deployment; however, these code changes are well known, and specific help is provided for each issue to assist in resolving it. 
 
   - Moderate complexity value typically represents about 80% of applications.
 
 **`Complex`**: The application uses a technology that does not have a direct equivalent in the new runtime and a new approach will need to be adopted. In our experience, only about 1 in 6 customers have these kinds of applications. 
 
   - Complex complexity value typically represents less than 1% of applications.

4. Click on **plantsByWebSphereee6.ear** which will expand the analysis

    ![A screenshot of a computer Description automatically generated](./images/media/image108.png)

    a. Scroll down, noting the groups or categories of analysis available
    
    - **Complexity rules**
    
    - **Required code changes**
    
    - **Issue details**

      - Critical

      - Warning

      - Informational
    
    - **Additional reports**

      - Technology report

      - Inventory report

      - Analysis reports


5.  View the **`Analysis Report`** for more detailed application information

    **Analysis Report**

    The **Detailed Migration Analysis Report** does a deep dive on the preferred migration target to help you understand any migration issues, like deprecated or removed APIs, Java SE version differences, and Java EE behavioral differences.

    a.  Click on the **`Analysis report`** link. 

    ![A screenshot of a computer Description automatically generated](./images/media/image109.png)

    b. The **Analysis report** will open in a new browser tab

    ![A screenshot of a computer Description automatically generated](./images/media/image110.png)
 

    c.  Note the **target**

    - Because the **source** was traditional WebSphere v8.5.5, which is Java EE 6, the target is also EE6.

    - Because the source was running Jave SE 8, the target is also set to Java SE 8.

    By default, Transformation advisor recommends the first step of modernization to Liberty that requires the minimal amount of change and effort.
 
    You may use the binary scanner with the `--ta` option if you want to assess the modernization effort to newer versions Java, Jave EE, or Jakarta EE. Running the binary scanner with the `-ta` option produces an archive file that can be loaded into Transformation Advisor for analysis.

    d.  Click on the **`Information`** label, to review the details of this item.

    ![A screenshot of a computer Description automatically generated](./images/media/image112.png)
 
    The **Information** rules provide migration related considerations. Be aware of these rules during testing. Many of these rules relate to connectivity to other resources that need to be considered during migration.

    e.  You may review the Information rules for PlantsByWebSphere migration.

    ![A screenshot of a computer Description automatically generated](./images/media/image113.png)


6.  Return to the **Cloud Transformation Advisor** browser tab that shows the **plantsbywebsphereee6.ear details** page. Then click on **`Inventory Report`**, which will open in a new browser tab.

    ![A screenshot of a computer Description automatically generated](./images/media/image114.png)

    a.  Scroll down and review the Inventory Report, which is especially useful in larger applications.

    The **Inventory report** provides a high-level inventory of the content and structure of each application, plus information about potential deployment problems and performance considerations.
 
    ![](./images/media/image115.png)

    b.  Scroll down to the bottom of the **Inventory report** and locate **`the Contained Archives`** section.

    The inventory report provides valuable insights into utility jars that are contained in the application. It also provides the ‘package’ name of the utility jar. This is extremely valuable to help determine what 3<sup>rd</sup> party utilities are used by the application.

    c.  Notice the PlantsByWebSphere application contains utility jar named **‘pbw-lib’jar**’. The archive package is “ibm.com.websphere’, indicating it is NOT a 3<sup>rd</sup> party utility jar.

    ![A screenshot of a computer Description automatically generated](./images/media/image116.png)

    d.  Return to the **Cloud Transformation Advisor** browser tab that shows the **plantsbywebsphereee6.ear details** page. Then click on **`Technology report`**, which will open in a new browser tab.

    The **Technology report** provides details on which editions of Liberty support the technologies used by the applications.
 
    ![A screenshot of a computer Description automatically generated](./images/media/image117.png)

    a.  Scroll down Technology Report, which can quickly help assess which Java EE API technologies that the PlantsByWebSphere application uses, and which editions of Liberty the APIS are available.

    ![A screenshot of a computer Description automatically generated](./images/media/image118.png)

    b. As you see from the report above, the APIs used in the PlantsByWebSphere application are available in ALL editions of Liberty.

</details>


