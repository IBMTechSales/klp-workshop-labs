# Proactive CVE protection for WebSphere with IBM WebSphere Automation

![banner](./lab1-media/media/image1.jpeg)

**Last updated:** November 2022

**Duration:** 90 mins

Need support? 

  - **Kevin Postreich** - kevinlp@us.ibm.com

  - **Yi Tang** - yitang@us.ibm.com


## Introduction to WebSphere Automation

[<span class="underline">IBM WebSphere Automation</span>](https://www.ibm.com/cloud/websphere-automation) is focused on delivering value into existing WebSphere Application Server (WAS) environments, helping administrators reduce the cost, effort, and risk of addressing common vulnerabilities, automating tasks, and remediating capacity incidents.

It removes manual toil so that your team can spend more time innovating while minimizing the cost of extending the life and maximizing the ROI of your WebSphere investments.

In conversations with customers, the same three concerns come up repeatedly. Organizations need to keep their IT estate secure and compliant, resilient to disruption and running optimally while reducing costs and maximizing ROI.

WebSphere Automation helps organizations gain visibility, operational efficiencies, and cost savings quickly by extending the life of WebSphere investments and giving teams time back to focus on unlocking new value and fixing the imbalance of pure maintenance versus innovation tasks.

  - WebSphere operators and administrators save time and embrace DevSecOps by implementing patches more efficiently on virtual and container environments to keep operations compliant and secure.

  - Enhance remediation capabilities with insights and recommendations to improve the speed and depth of understanding of outages and anomalies as they occur.

  - Augment the operational experience with access to simplified and consolidated information that enables teams to act.

With WebSphere Automation, security, business efficiency and resiliency become standard. IBM can meet you wherever you are in your optimization and automation journeys to help you quickly deliver value and increase ROI, all while laying a solid automation foundation to support future growth.

IBM WebSphere Automation is a stand-alone product that runs on RedHat OpenShift. Entitlement to RedHat OpenShift is included with IBM WebSphere Automation but must be installed separately. As part of IBM Automation platform, IBM WebSphere Automation includes containerized components and common software services on top of a common automation layer, to manage WebSphere’s incidents, hybrid applications, and cost with complete observability, governance, and compliance.


Deploy virtually anywhere through containers supported by Red Hat® OpenShift® software, on IBM Cloud®, on essentially any existing infrastructure on-premises, or through private and public clouds. Use only the capabilities you need with a fully modular approach that’s designed to be easy to consume.

## Business Context

You are a WebSphere Administrator, part of a WebSphere Operations Team responsible for maintaining security compliance of the WebSphere estate in the enterprise. A typical “as-is” process for maintaining security compliance for WebSphere environments is depicted below.

**Today (as-is):**

1.  IBM sends generic “FLASH” to indicate a new WAS security bulletin.

2.  You subscribe and receive IBM Security Bulletins to be aware about vulnerabilities, its potential impact, severity, and recommended solutions.

3.  Generally, WAS inventory is maintained in spreadsheets.

4.  Based on that, you check if this CVE applies to your managed inventory (Spreadsheet)

5.  You determine if an APAR / Fix Pack upgrade should be applied to    existing environment

6.  You deploy the fix to the impacted environments

7.  You update the WAS inventory (Spreadsheet) and provide up-to-date    reports to audit and compliance teams

As is, your inventory is a spreadsheet, containing all information about your servers, such as the versions of the installed servers, which operating system they're installed on, and iFixes which have been applied, etc

![](./lab1-media/media/image2.png)

Currently, this is a very manual, time-consuming process, and you'd like to automate this process to direct valuable time and resource elsewhere. This is where **IBM WebSphere Automation** will help\!

You would like to have:

  - **Management dashboard:** Consolidated dashboard increases awareness and response time to common vulnerabilities and exposures (CVEs).

  - **Automated vulnerability tracking:** Let WebSphere Automation track new security bulletins across your existing traditional WebSphere and Liberty environments, on virtual machines or containers.

  - **Contextual notifications:** Receive security bulletin notifications only when new vulnerabilities affect the environment you manage, reducing noise and interruptions to the WebSphere operations team.

  - **Shared, live visibility to key stakeholders:** WebSphere operators and security compliance teams can see the real-time security posture of the WebSphere estate, accelerating action and minimizing the risk of miscommunication.

  - **Fix History:** A complete audit trail for when vulnerabilities are detected, and when patches or upgrades are applied to resolve issues
  
  - **Automated fix installation of Fixpaks and iFixes** to your WebSphere and Liberty servers, directly from the IBM WebSphere Automation dashboard. 

In this lab, you use the IBM WebSphere Automation to secure operations to reduce risk and meet compliance.

At the end of this lab, you will be able to connect teams with the most relevant information through a single dashboard. This enables you to discover, analyze and remediate common vulnerabilities and exposures across instances. 

You will gain the necessary insights into **Fix History** to help operations teams demonstrate they are meeting patching SLAs, which are very common in large enterprises. Furthermore, this information can be exported to a CSV file to be shared amongst the broader team.

You will use the **one-click Fix Deployment** capability of WebSphere Automation 1.4 to automatically determine which APARs and interim fixes to install to resolve a specific vulnerability, and to download and deploy the fixes. 


![](./lab1-media/media/image3.png)

## Accessing the environment

If you are doing this lab as part of an instructor led workshop (virtual or face to face), an environment has already been provisioned for you. The instructor will provide the details for accessing the lab environment.

Otherwise, you will need to reserve an environment for the lab. You can obtain one here. Follow the on-screen instructions for the “**Reserve now**” option.

<https://techzone.ibm.com/my/reservations/create/60da2c20e2cb7a001f656575>


1.  Access the lab environment from your web browser. 
    
    a.  When the demo environment is provisioned, right-mouse click on the **Published Service** link and open the URL in a new private or incognito browser window, as described below:  
    
    - If using **Google Chrome** browser, select **"Open link in incognito window"** from the context menu. 

      ![](./lab1-media/media/chrome-incognito.png)
    
    - If using **Firefox** browser, select **"Open link in new private window"** from the context menu. 

      ![](./lab1-media/media/chrome-incognito.png)

    b. Click on the **"vnc.html"** link to open the lab environment through the **noVNC** interface. 

      ![](./lab1-media/media/vnc-link.png)

 
    c. Click the **Connect** button 
    
      ![](./lab1-media/media/vnc-connect.png)

    d. Enter the password as:  **passw0rd**. Then click the **Send Credentials** button to access the lab environment. 

    > Note: That is a numeric zero in passw0rd  

      ![](./lab1-media/media/vnc-password.png)


    IBM WebSphere Automation is pre-installed in the OCP cluster hosted on the VMs.
	 
	 <br>
	 

3. The Published Service provides access to the **STUDENT** VM through the noVNC interface for the lab environment. 

    All the WebSphere **images** and **iFixes** required for the lab are pre-installed on the VM:

    - /opt/IBM/WebSphere/Liberty20009/

    - /opt/IBM/WebSphere/AppServer9057/

    <br>

	
5.  Once you log in to the Student VM you will see the Desktop, which contains all the programs that you will be using (browsers, terminal, etc.)

    The login credentials for the **STUDENT”** image is:
 
     - User ID: **ibmuser**

     - Password: **engageibm**
	 
	 <br>
 
     ![student vm screen](./lab1-media/media/image8.png)
	 
	 <br>

## Tips for working the in lab environment     

1.  You can use your Browsers **zoom in** and **zoom out** options to resize the virtual desktop to fit your screen.

    The examples below are using Firefox and Chrome browsers. 

    - Firefox example: 
    
    ![fit to window](./lab1-media/media/zoom.png)

    - Chrome example: 
      
     ![fit to window](./lab1-media/media/zoom-chrome.png)


2.  You can copy / paste text from the lab guide into the lab environment using the clipboard in the noVNC viewer. 
   
    a. Copy the text from the lab guide that you want to paste into the lab environment
    
    b. Click the **Clipboard** icon and **paste** the text into the noVNC clipboard

    ![fit to window](./lab1-media/media/paste.png)
    
    c. Paste the tect into the VM, such as to a terminal window, browser window, etc. 

    d. Click on the **clipboard** icon agian to close the clipboard

    > **NOTE:** Sometimes pasting into a Terminal window in the VM does not work consistently. In this case you might try again, or paste the text into a **Text Editor** in the VM, and then paste it into the Terminal window in the VM. 

3. An alternative to using the noVNC Copy / Paste option, you may considr openeing the lab guide in a web browser inside of the VM. Using this method, you can easily copy / paste text from the lab guide wihout having to use the noVNC clipboard. 


    <br>

## How IBM WebSphere Automation automatically manages your WebSphere and Liberty server security posture 

WebSphere administrators register their WebSphere Application Server or Liberty servers with WebSphere Automation. 

The WebSphere Automation **vulnerability manager** makes an assessment of the security compliance status of each server. Common vulnerabilities or exposures (CVEs) for each server are displayed in the WebSphere Automation UI in an interactive list, and each server is assessed a risk level. Administrators can learn more about the pertinent CVEs, plan their response, and complete the application of the required security fixes to their managed servers using the WebSphere Automation UI.

When the IBM Product Security Incident Response Team (PSIRT) publishes new or updated security bulletins, the WebSphere Automation **CVE/PSIRT monitor** detects them and collects the data about the CVEs from the bulletins. The WebSphere Automation vulnerability manager checks the applicability of the new CVEs to the registered servers. If exposures are found, the WebSphere Automation vulnerability notifier sends email notifications to a customizable list of addresses that new vulnerabilities exist.

After defining an exposure mitigation plan, administrators then use the WebSphere Automation UI to select published fix packs or interim fixes to repair vulnerabilities. During the fix installation process, WebSphere Automation requests the selected fix from IBM Fix Central, stores it in the Kafka data store, and then installs it on the indicated server.

## Clone the Git repository used for this lab and explore the contents

1. Clone the GitHub repo with the lab artifacts, then run the following command on your terminal:

    a. Open a Terminal window on the VM, and ensure you are in the home directory of the user “ibmuser”

        cd /home/ibmuser


    b. From the terminal window, run the following command to clone the repo:

        git clone https://github.com/IBMTechSales/WAS-Automation-LabFiles.git

    ![](./lab1-media/media/clonerepo.png)


## REQUIRED: WebSphere Automation setup

The Fix Deployment capability of WebSphere Automation 1.4 delivers automation that determines which APARs and interim fixes resolve a specific vulnerability, and enables one-click download and deploy of fixes. This capability augments the existing automated vulnerability assessment and fix history tracking.

To apply security fixes from managed servers, both WebSphere Automation and the WebSphere servers must be properly configured to communicate by using SSH. 

The following configuration tasks must be completed to use the Fix Deployment features in IBM WebSphere Automation.  
You will do these steps in this section of the lab, to become familiar with configuring IBM WebSphere Automation. 

  - Create the **ssh key**
  - Copy the key over to the WAS server to be monitored
  - Create the **wsa-ansible** secret that contains the key
  - Create the **known_hosts** file
  - Create the **wsa-secure-fixcentral-creds** secret that contains the IBM ID credentials to access IBM Fix Central

1. Login to OpenShift CLI

    a. Open a new Terminal window in the VM
  
    b.  Type `oc login -u ibmadmin -p engageibm` to login to OpenShift. 
    
    Use the following credentials to login: 
    > 
    > username: ibmadmin

    > password: engageibm

     ![](./lab1-media/media/image70.png)

     <br/>

2.  Type `oc project websphere-automation` to ensure you are working in the **websphere-automation** project

    ![](./lab1-media/media/image71.png)


3. Type `oc get wsa` to verify that IBM WebSphere Automation is READY in your environment. 

   **Note:** The output must state that the WSA resource is **ready** before you can continue with the lab.  

    ![](./lab1-media/media/image72.png)



4. Create the **ssh key** that is used for secure communication between WebSphere Automation and the WebSphere servers

        cd /home/ibmuser
        
        ssh-keygen -f ~/.ssh/wsa

    a. When prompted, enter the password as: **passw0rd** 
    
    > **Note:** That is a numeric zero in **passw0rd**

    > This is the password you are assigning for the new ssh key that is being reated

    ![](./lab1-media/media/image73.png)

    Additional documentation can be found here: [https://www.ibm.com/docs/en/ws-automation?topic=servers-setting-up-ssh-linux-unix](https://www.ibm.com/docs/en/ws-automation?topic=servers-setting-up-ssh-linux-unix)


5.	Copy the ssh key over to the WAS server to be monitored. In this lab the following command is used:

        ssh-copy-id -i ~/.ssh/wsa ibmuser@student.demo.ibmdte.net

    a. When prompted to continue connection, type `yes`

    b. When prompted for the password for ibmuser@student.demo.ibmdte.net, type `engageibm` as the password. 

    Ensure the message indicates that 1 key was **added** to the users key store. 

    **Note** This password is for the operating system user: **"ibmuser"** so that the ssh key can be securly copied.  

    ![](./lab1-media/media/image74.png)

6.	Test the login to the WAS server from the WebSphere Automation server via ssh key with the key passphrase from the ssh key, which is **passw0rd** 

        ssh -i ~/.ssh/wsa ibmuser@student.demo.ibmdte.net

    ![](./lab1-media/media/image76.png)

    a. Type `whoami` from the command line and verify the user is **ibmuser**

    ![](./lab1-media/media/image75.png)

     b. Type `exit` from the command line to close the ssh connection

      ![](./lab1-media/media/image77.png)


7. Create the **wsa-ansible** secret that contains the key

        oc create secret generic wsa-ansible \
        --from-literal=ansible_user=ibmuser \
        --from-literal=ansible_port=22 \
        --from-file=ssh_private_key_file=/home/ibmuser/.ssh/wsa \
        --from-literal=ssh_private_key_password=passw0rd

    **Note:** The "wsa-ansible" secret contains the connection information required for WebSphere Automation to connect to the WebSphere server used in this lab. This connection is required for WebSphere Automation to ssh into WebSphere and execute Ansible playbooks to install WebSphere iFixes and Fixpacks. 

    ![](./lab1-media/media/image78.png)

    Additional documentation can be found here: [https://www.ibm.com/docs/en/ws-automation?topic=servers-setting-up-websphere-automation-ssh](https://www.ibm.com/docs/en/ws-automation?topic=servers-setting-up-websphere-automation-ssh)


8. Create a **known-hosts** file which will configure WebSphere Automation with a list known hosts and their public keys that WebSphere Automation will trust. 

        ssh-keyscan student.demo.ibmdte.net >> /home/ibmuser/wsa_known_hosts
        
    ![](./lab1-media/media/image79.png)
    
    Additional documentation can be found here: [https://www.ibm.com/docs/en/ws-automation?topic=servers-setting-up-websphere-automation-ssh](https://www.ibm.com/docs/en/ws-automation?topic=servers-setting-up-websphere-automation-ssh)

9. Create the **wsa-ansible_known_hosts configMap**, using the known-hosts file you just created. 

        oc create configmap wsa-ansible-known-hosts --from-file=known_hosts=/home/ibmuser/wsa_known_hosts
  
    ![](./lab1-media/media/image80.png)



10. Test the ssh connection from IBM WebSphere Automation to WAS environment, using the following commands: 

        MANAGER_POD=$(oc get pod -l app.kubernetes.io/component=runbook-manager -o name | head -n 1)

        oc rsh $MANAGER_POD runcli testConnection student.demo.ibmdte.net linux

    a. Run the `oc log` command that is outputted from the "**oc rsh**" command. 

       **Example:**   
       oc logs --tail=100 -l job-name=test-connection-######     

       >  **Note:** Run the command multiple times until you see the exected output as shown below. At first, you will see messages "**waiting to start**", which can take a minute or two.    

    ![](./lab1-media/media/image94.png)

    b. A successful connection should look similar to this output: 

    ![](./lab1-media/media/image95.png)

     ![](./lab1-media/media/image95-cont.png)


11. Verify the Kubernetes secret named **wsa-secure-fixcentral-creds** has been created in the lab environment. 

        oc get secret | grep wsa-secure-fixcentral-creds

    **Note:** This secret contains the information required for WebSphere Automation to access IBM Fix Central to fetch WebSphere and Libery fixes.   

    **Note:** The wsa-secure-fixcentral-creds secret has already been created for you in the lab environment. 
    
    ![](./lab1-media/media/fixcentral-secret.png)    
 
    When the secret is created, WebSphere Automation strarts two additional pods in OpenShift that are responsible for downloading WebSphere and Liberty fixes from IBM Fix Central and installing the fixes into WebSphere and Liberty servers that are registered with WebSphere Automation.  

      - wsa-secure-fix-manager-*
      - wsa-secure-installation-manager-*


  
12. Ensure the following two pods mentioned above are created and started, using the **oc get pods** command below: 
 
    - wsa-secure-fix-manager-*
    - wsa-secure-installation-manager-*

    <br/>  
   
    ```   
    oc get pods | grep '\<fix\>\|installation'
    ```
    
    > **Note:** These pods are only created and started once the **wsa-secure-fixcentral-creds** secret is created in the OCP cluster. If the credentials in the secret are updated, the wsa-secure-fix-manager-* pod will automatically be restarted by OpenShift based on the updated credentials. 


    ![](./lab1-media/media/image82.png)


    The **wsa-secure-fix-manager-\*** pod is responsible for downloading the fixpaks and iFixes from IBM Fix Central, using the credentials you provided in the **wsa-secure-fixcentral-creds** secret. 

    The **wsa-secure-installation-manager-\*** pod is responsible for running the Ansible playbooks to install the fixpaks or iFixes into the registered WebSphere or Liberty servers. 

    <br/>

> **You have now completed the custom configuration required for this lab.**

<br/>


## Part 1: Receiving vulnerability notifications

#### Accessing the WebSphere Automation UI

A WebSphere administrator sets up WebSphere Automation by registering and configuring WebSphere Application Servers and WebSphere Liberty servers for vulnerability tracking and by configuring email notifications.

WebSphere administrators can also view the results of vulnerability assessment in WebSphere Automation to plan their response for the WebSphere Application Server and WebSphere Liberty servers that they manage.

For this lab, WebSphere Automation is pre-installed on an OCP cluster. You have your individual WebSphere Automation installation. Let’s access your environment.

1.  On the *Student VM*, open a browser and enter the following URL (there is a WebSphere Automation link on bookmark toolbar):
    
    **Note:** It takes about **10-15 minutes** for the environment to start and stabilize once it has started. If you encounter "**Secure Connection Failed**" or  "**502 Gateway Error**" accessing the WebSphere Automation URL, please wait a few minutes and retry. 
    
	<a href="https://cpd-websphere-automation.apps.demo.ibmdte.net/zen/#/homepage"><span class="underline">https://cpd-websphere-automation.apps.demo.ibmdte.net/zen/#/homepage</span></a>


    ![opening browser](./lab1-media/media/image10.png)
	
	<br>

2.  On the login page, select the **OpenShift authentication** as the    authentication type.

    **Note:** If using Firefox, enlarge the browser window until you see the graphics, as illustrated below. 

     ![openshift authentication](./lab1-media/media/image11.png)
 
     **Note:** If necessary, accept all the warnings and certificates.  Depending on your browser, you might have to scroll down to permit
 access.

    <br>
	
3. Select **htpasswd** as the login selection
  
    ![ocp ui login](./lab1-media/media/image12.png)
	
	<br>

4.  Enter **ibmadmin** as username and **engageibm** as password. And click **Log In**.

    ![login page](./lab1-media/media/image13.png)

    <br/>

     If the **"IBM Cloudpak | Administration"** page appears, navigate to the **IBM Automation page** using the sub-steps below. Otherwise continue with the next step, as you should already be at the **IBM Automation** page.  

    ![cloudpack](./lab1-media/media/x-ibm-cloudpak.png)

    a. In the upper right corner, click on the **cloudpak switcher** icon

    ![grid](./lab1-media/media/x-switch-automation.png)

    b. Select **IBM Automation (WebSphere Automation)** from the menu

    ![select-it](./lab1-media/media/x-select-automation.png)

5.  At this point, you should be at the ***IBM Automation Welcome page*** 

    ![welcome page](./lab1-media/media/image14.png)

    <br>
	
6.  View the Application Runtimes that have been registered with IBM Automation

    a. Click on the**Navigation Menu** icon located at the upper left corner of the page

      ![notification config](./lab1-media/media/image15.png)

    b. Click **Operate**, and then click **Application runtimes**.
  
    Application Runtimes represent the Traditional WebSphere and WebSphere Liberty servers that have been registered with IBM Automation

      ![notification config](./lab1-media/media/image16.png)

      <br/>

7. The **Application runtimes – Security** page appears. There should be no data since there not any WebSphere / Liberty servers registered yet.

    |         |           |  
    | ------------- |:-------------|
    | ![](./lab1-media/media/image47.png?cropResize=50,50)   | <strong>IMPORTANT:</strong> <br><br> If any of the labels on the page don't display properly, refresh the browser window using the web browser refresh icon.  <br><br> For example: The label for the **Fix management** tab may display as **!Fix management!** the first time. Refreshing the browser window will result in the labels being displayed correctly.  <br><br> Note: This is only a one-time action. 

     ![App runtime](./lab1-media/media/image17.png)

    <br/>

8.  Before you start to register servers to the Dashboard, you need to configure an email to received notifications about CVEs.
    
    a. Select the **Notifications** menu item from the navigation list.
    
    b. Click on the **Email addresses** tab, to add your email address where notifications of new CVEs will be delivered

    ![notification config](./lab1-media/media/image18.png)
	
	<br>
	
9.  The Email server configuration is pre-configured for this lab. You only need to add your personal email to receive notifications of new security vulnerabilities.
    
    a. Click the **Add** button
    
    b. Enter your **email address**
    
    c. Click **Save**

    ![notification email](./lab1-media/media/image19.png)

**Great\!** Your email is configured to receive security notifications.

In the next section, you will register servers to WebSphere Automation.

<br>

## PART 2: Getting configuration parameters needed to register WebSphere / Liberty servers to IBM Automation

Add each of your WebSphere Application Server and WebSphere Liberty servers to WebSphere Automation by registering them with the **usage metering** service.

To register your application servers with the usage metering service in WebSphere Automation, you must configure the usage metering feature in each application server. To configure the usage metering feature in each of your application servers, you must obtain the following usage metering details:

  - **URL**: The URL of the usage metering service in WebSphere Automation. This service registers WebSphere Application Server and Liberty servers with WebSphere Automation so that you can track security vulnerabilities.

  - **API Key**: The token used to authenticate the WebSphere Application Server and Liberty servers during the registration process.

  - **Usage metering certificate**: The certificate that contains the public key. This key allows a Liberty server that is registering with WebSphere Automation to do an SSL handshake with the metering service.

In this section, you will get these configuration parameters that will be used to register application servers.


1.  Return to the desktop and open a new **terminal** window.

    ![open terminal](./lab1-media/media/image20.png)
	
	<br>

2.  Login to the OpenShift Cluster using the oc CLI command shown below, if you are not already logged in:

        oc login --username=ibmadmin --password=engageibm --insecure-skip-tls-verify=true --server=https://api.demo.ibmdte.net:6443

       ![oc login](./lab1-media/media/image21.png)
	
	<br>

3.  Make sure that the project you are working on is **websphere-automation**:

        oc project websphere-automation

    ![](./lab1-media/media/image22.png)

    <br/>

4.  Use the **oc** command to get the URL of the usage metering service in WebSphere Automation and save it to a file  “/opt/IBM/WebSphere/metering-url.txt”

        echo https://$(oc get route cpd -n websphere-automation -o jsonpath='{.spec.host}')/websphereauto/meteringapi > /opt/IBM/WebSphere/metering-url.txt

5.  View the contents of the saved file to ensure the URL was captured.

        cat /opt/IBM/WebSphere/metering-url.txt

    ![](./lab1-media/media/image23.png)
	
	<br>

6.  Get the **api-key** that will be used to authenticate the WebSphere Application Server and Liberty servers during the registration process. Save it to a file named “/opt/IBM/WebSphere/api-key.txt”

        oc -n websphere-automation get secret wsa-secure-metering-apis-encrypted-tokens -o jsonpath='{.data.wsa-secure-metering-apis-sa}' | base64 -d > /opt/IBM/WebSphere/api-key.txt; echo >> /opt/IBM/WebSphere/api-key.txt

7.  View the contents of the saved file to ensure the api-key (token) was captured.

        cat /opt/IBM/WebSphere/api-key.txt

     ![](./lab1-media/media/image24.png)
	 
	 <br>

8.  Finally, get the Server certificate that is used for SSL handshake between the **Liberty servers** and IBM Automation, and save it to a file named “/opt/IBM/WebSphere/cacert.pem”

        oc get secret external-tls-secret -n websphere-automation -o jsonpath='{.data.cert\.crt}' | base64 -d > /opt/IBM/WebSphere/cacert.pem

9. View the contents of the saved file to ensure the api-key (token) was captured.

        cat /opt/IBM/WebSphere/cacert.pem

    ![](./lab1-media/media/image25.png)

**Great\!** Now you have all the configuration parameters necessary to register the application servers with the usage metering service in WebSphere Automation.


|         |           |  
| ------------- |:-------------|
| ![](./lab1-media/media/image4.png?cropResize=100,100)   | <strong>For more information:</strong> <br> For additional details, view the section titled: <strong>Setting up security monitoring</strong> in the WebSphere Automation on-line documentation, or download it as a PDF from this URL: [https://www.ibm.com/docs/en/ws-automation](https://www.ibm.com/docs/en/ws-automation)|


In the next section, you register your first server in WebSphere Automation.

<br>

## Part 3: Working with Traditional WebSphere servers with IBM Automation – Security Vulnerabilities

You can view the security vulnerability status of each server in your inventory from the Security page in the WebSphere Automation UI. 
For each server, the numerical risk level in the range 0 - 10 represents the highest CVSS value of its CVEs.

To set up security monitoring, add each of your WebSphere Application Server servers and WebSphere Liberty servers to WebSphere Automation by registering them with the usage metering service. 

You can then track security vulnerabilities for your servers from a single user interface (UI) in WebSphere Automation.

To register your application servers with the usage metering service in WebSphere Automation, you must configure the usage metering feature in each application server. 

In Part 3 of the lab, you will perform the following task for WebSphere Traditional servers in IBM Automation. 

1.	**Register a WebSphere traditional application server** (tWAS), version 9.0.5.7 to IBM Automation, using the URL, API Key, and usage Metering Certificate that you gathered in the previous section of the lab. 

    Once registered, you will immediately see the list of all unresolved CVEs and applied iFixes for the server. 


2.	**Resolve the known Log4J vulnerabilities in the WebSphere traditional application server**, by using the built-in capabilities in IBM WebSphere Automation to prepare and install the recommended iFix documented in the security bulletin for this vulnerability. 

    Once the iFix is applied, IBM Automation immediately updates the security information for that application server, showing that the critical Log4J vulnerabilities have been patched. 

    
 <br>

## 3.1 Register traditional WebSphere (tWAS) v9.0.5.7

In this section, you configure a traditional WebSphere Application Server to your WebSphere Automation dashboard. With traditional WebSphere, you use the wsadmin script to configure the usage metering service.


1.  First, let’s start the traditional WebSphere (tWAS) 9.0.5.7 server. Return to the terminal window and execute the command below:

        /opt/IBM/WebSphere/AppServer9057/bin/startServer.sh tWAS_9057_server

    ![](./lab1-media/media/image26.png)
	
	<br>

2.  Configure usage-metering using the wsadmin script below:

    The wsadmin command invokes a Python script named configuretWasUsageMetering.py. The script requires the same **url** and **apiKey** that you gathered earlier from the IBM automation environment. These parameters are captured from the text files that you saved earlier in the lab and inserted into the url and apiKey parameters.

        /opt/IBM/WebSphere/AppServer9057/bin/wsadmin.sh -f /api-usagemetering/scripts/configuretWasUsageMetering.py url=$(cat /opt/IBM/WebSphere/metering-url.txt) apiKey=$(cat /opt/IBM/WebSphere/api-key.txt) trustStorePassword=th1nkpassword

    The script should run successfully as illustrated below.

    ![](./lab1-media/media/image27.png)
	
	<br>

    Great, you first traditional WAS server is configured. Let’s check the WebSphere Automation dashboard. 

    </br>

3.	Go back to the browser, navigate to the **Security -> Servers** dashboard, and confirm that the tWAS v9.0.5.7 server was registered in IBM Automation.

    **Note:** It may take 15 - 30 seconds for the server to be displayed in the WebSphere Automation UI. 

    ![dashboard tWAS1](./lab1-media/media/image28.png)
 
    You should see that this server Risk Level is 10.0, with (+## more) unresolved CVEs. 

    **Note:** Somewhere at the top of that list is the much talked about LOG4J vulnerability, known as CVE-2021-44228, with a critical score of 10.  
 


	|         |           |  
    | ------------- |:-------------|
    | ![](./lab1-media/media/image4.png?cropResize=50,50)   | <strong>TIP:</strong> <br><strong>Note:</strong> New vulnerabilities are discovered constantly, so the number of CVEs discovered may be different than illustrated.
    
    <br>
 
4.  Check your email. A mail notification was sent showing new vulnerabilities for the registered application server.

    ![](./lab1-media/media/image29.png)

	|         |           |  
    | ------------- |:-------------|
    | ![](./lab1-media/media/image4.png?cropResize=100,100)   | <strong>Information:</strong> <br>In the email message, there will be a link that would redirect to the console to show more details on the vulnerability. However, Because of network restrictions in the lab environment, this link will not work. 

    <br>


## 3.2 Review CVE-2021-44228 to understand the remediation options for the LOG4J vulnerability

In this section, you will review the details of the critical (10.0) **CVE-2021-44228** CVE and determine the appropriate remediation options to resolve the CVE.

In this lab, you will apply the iFix that is documented in the IBM security bulletin to resolve the Log4J vulnerability in the traditional WebSphere 9.0.5.7 server, as reported in **CVE-2021-44228**.

1.  Notice in the IBM Automation console that tWAS 9.0.5.7 is vulnerable to CVE-2021-44228.

    ![](./lab1-media/media/imagev3-32.png)

2.  Click on **(+ \<NUMBER\> more)** to expand the list of unresolved CVEs. 
    
    IBM Automation also detected the additional unresolved CVEs related to the Log4J vulnerability, which are highlighted below and documented in the security bulletin.

    ![](./lab1-media/media/imagev3-33.png)
 
    As noted in the IBM security bulletin, which you will explore next, there are three CVEs related to the LOG4J vulnerability. A subset are highlighted below in the list of Unresolved CVEs for this WebSphere server.
 
    ![](./lab1-media/media/imagev3-34.png)

3.  After viewing the complete list of unresolved CVEs for the 9057 server, close the “**Unresolved CVEs**” view.

4.  View the security bulletin(s) for the **CVE-2021-44228** Log4J vulnerability


    a.  Click on **CVE-2021-44228** link under the 9057 servers **Unresolved CVEs** column

    ![](./lab1-media/media/imagev3-35.png)

    From the CVE Information page that is displayed, you can see important details about this CVE.

    <u>Here you can see the following details:</u>

    - Risk Level “Critical (10)

    - Date the vulnerability was detected in the “**registered**” server

    - Number of days the 9057 WebSphere server has been “**exposed**” to the vulnerability

    - List of affected servers that are registered with IBM WebSphere Automation.

    - Link to the IBM security bulletin for the CVE

    > **Note:** The **Days Exposed** and **date detected** are expressed from the point of time that the server is registered with IBM WebSphere Automation.

    b.  Click on the **View** Link to view the IBM security bulletin for this CVE.

    ![](./lab1-media/media/imagev3-36.png)

    c.  The security bulletin is displayed in a new browser tab.

    d.  Review the security bulletin to learn about the vulnerability, versions of WebSphere affected, and options for remediating the vulnerability.

    ![](./lab1-media/media/imagev3-37.png)
 
    As you see, IBM WebSphere Automation makes it easy for you to get to the related security bulletins directly from the CVE information within the WebSphere Automation dashboard so you can have all the details and remediation options at your fingertips

    > **Note:** The IBM Security Bulletin associated with this vulnerability documents the options for remediating CVE-2021-44228 and related CVEs (CVE-2021-4104 and CVE-2021-45046)

    ![](./lab1-media/media/image32.png)

    You can read the security bulletins from the **ibm.com/support** pages. You may be prompted to login using your IBM ID to access the content:

    <https://www.ibm.com/support/pages/node/6525706>

    

5.  Return to the Browser tab that contains the IBM WebSphere Automation dashboard. Then return to the “**Security**” view


    ![](./lab1-media/media/imagev3-39.png)



## 3.3 Update tWAS server v9.0.5.7 to fix the LOG4J vulnerability

Automated installation of WebSphere and Liberty fixpaks and iFixes was introduced into IBM WebSphere Automation in version 1.4

You can automatically deploy fixes to servers in your managed server inventory through the WebSphere Automation UI. You must have a user profile with at least the **Modify WebSphere inventory** permission, and the servers that you want to fix must be registered and configured for fix installation with WebSphere Automation.

In this section, you will leverage the built-in capabilities in IBM WebSphere Automation to automatically prepare and install the recommended iFix to the traditional WebSphere 9.0.5.7 server to remove the Log4J vulnerability reported as **CVE-2021-44228**.

There are a couple of additional components now included in the product to support this use case. 

  - **Fix manager**
    
    The WebSphere Automation fix manager uses credentials that you provide to access IBM Fix Central to request fixes. Fixes are fetched and stored in the file storage defined for the websphereSecure custom resource. The fix manager manages the storage space according to frequency of use, deleting older fixes to make room for more recently requested fixes.

  - **Installation manager**
  
    The WebSphere Automation installation manager communicates with the registered server using the administrator privileges that you provide. When you initiate the installation of a fix, the installation manager ensures that the target server has sufficient space for the fix, transfers the fix to the target server, installs the fix, and creates a log file of the steps taken. If you request a backup of the server environment as part of the fix installation, the installation manager checks for sufficient disk space on the server, and creates an archive of the Installation Manager, Installation Manager data, and WebSphere Application Server or WebSphere Application Server Liberty server installation directories.

### 3.3.1 Navigate to the Server and unresolved CVE to fix

OK, let's apply the recommended iFix to resolve the CVE-2021-44228 vulnerability! 

1.  Notice in the IBM Automation console that tWAS 9.0.5.7 is vulnerable to **CVE-2021-44228**.

    ![CVE 2021 44228](./lab1-media/media/image30.png)

 
    **Note:** The IBM Security Bulletin associated with this vulnerability documents the options for remediating **CVE-2021-44228** and related CVEs (**CVE-2021-4104** and **CVE-2021-45046**) 

    ![security bulletin info](./lab1-media/media/image32.png)

 
    You will fix it by applying the recommended iFix. (PH42728)

    <br>

2. Click on **tWAS_9057_server** to view the list of CVEs / vulnerabilities
 
     ![](./lab1-media/media/image83.png)
    

    The displayed list shows both "resolved" and "unresolved" CVEs for the selected server. It also shows the "Risk Level" to identify the severity of the vulnerabilities, and the number of days the server has been exposed to the vulnerability. 

    ![](./lab1-media/media/image84-a.png)
 
3. Click on CVE-2021-44228 to view the CVE information and affected servers

    ![](./lab1-media/media/image84.png)

    On the CVE Information page for CVE-2021-44228, a list of **Affected Servers** is displayed along with the option to **Prepare fix**. 

    **DO NOT prepare fix yet.** You will do that in the following steps. 

    ![](./lab1-media/media/image85.png)
    

### 3.3.2 Prepare Fix 

The **Fix Deployment** capability of WebSphere Automation delivers automation that determines which APARs and interim fixes resolve a specific vulnerability, and enables one-click download and deploy of fixes. 

Selection of the wanted iFix or published fix pack is exposed through the **Prepare fix** dialog, which can be found in the CVE details view. Simply select the target server to patch and the dialog shows all the affected servers known to WebSphere Automation, and lets you select the wanted fix.

After you select the fix, WebSphere Automation provides two options: 

  - **Fetch fix:** 

    If installation should be deferred to a later time, then the **Fetch fix** option causes the fix to be downloaded and stored for later use. 
  
    This can be preferable for fix packs because those are significantly larger than interim fixes, especially for traditional WebSphere fix packs.
  
  - **Fetch then install fix:** 

    The **Fetch then install fix** option automatically downloads the fix and then installs it after the download is complete. 
  
    Previously fetched fixes are stored within WebSphere Automation for immediate reuse. 

    <br/>  

    > Now you will download and install the recommended iFix for the CVE into the tWAS_9057_server    

1. Download the recommended iFix to resolve the CVE
 
    Next to each affected server, there is an option to "**Prepare Fix**". 
    
    a. Click the **Prepare fix** link next to tWAS_9057_server for CVE-2021-44228

    ![](./lab1-media/media/image86.png)

    b. From the **Select fix** page, select the **PH42728** iFix. Then click the **Fetch fix** button. 

    
    |         |           |  
    | ------------- |:-------------|
    | ![](./lab1-media/media/image47.png?cropResize=50,50)   | <strong>IMPORTANT:</strong> <br><br> If the **PH42728** iFix is not listed under the **"resolution"** column as illustrated in the screen shot below, give WebSphere Automation a few more minutes to load the fix meta-data into its database.  <br><br>  To avoid overloading WebSphere Automation, it only loads the meta-data for unresolved CVEs when a WebSphere or Liberty server is registered. And, WSA only loads the meta-data for the unresolved CVEs of the registered server(s). <br><br>  WSA does this using a background process that spins up several threads. Because there are more unresolved CVEs than number of threads loading the meta-data,  it will take a WSA a couple of iterations to load all of the meta-data for the fixes. <br><br>   Typically, the process will complete within 8-10 minutes after the server has been registered with WebSphere Automation. <br><br> WSA will continue to load fix meta-data as new unresolved CVEs are detected for registered servers.

    ![](./lab1-media/media/image87.png)


    This action will cause WebSphere Automation to fetch the fix from IBM Fix Central, using the credentials that you configured at the beginning of the lab. 

    The **Status** field automatically updates as the action progresses. 

    ![](./lab1-media/media/image88.png)

    When the fix has been successfully fetched from IBM Fix Central, the status will change to **Ready to Install**.

    ![](./lab1-media/media/image89.png)


2. Install the fix to resolve the CVE

    Once the fix has been successfully fetched, the action to **Install the fix** is made available next to the affected server.

    a. Click on the **Install fix** action next to the tWAS_9057_server    

    ![](./lab1-media/media/image90.png)

    b. When prompted to confirm the installation, click **Proceed**

    > **Note:** For this lab, "Create backup" option should be set to "**off**".


    ![](./lab1-media/media/image91.png)


	|         |           |  
    | ------------- |:-------------|
    | ![](./lab1-media/media/image4.png?cropResize=100,100)   | <strong>Alert:</strong> <br>In this lab environment, when the Install Fix action is initiated, a pop-up error dialog may be displayed that may be ignored!<br/> <br/>![](./lab1-media/media/image96.png) <br/> If the "Status" field has not changed to "Failed", let the installation proceed to completion. 

    
    c. The installation may take 5 or 6 minutes to complete. Wait until you see the status change to "**Installation complete**"

    ![](./lab1-media/media/image92.png)


    > **TROUBLESHOOTING TIP**
    > 
    > If the installation fails, click on the **ID** of the action, and view the runbook.log output file. 
    >
    >  Common errors we have seen reported in the log is the ssh key and/or the wsa-ansible secret not properly configured in the environment.  
    >
    > ![](./lab1-media/media/image93.png)
    >
    > ![](./lab1-media/media/image97.png)


**Congratulations!** You have successfully applied the iFix to resolve the critical CVE on the tWAS_9057_server. 

In the next section of the lab, you will review the updated security posture and fix history of the WebSphere server. 

<br>


## 3.4 View the security posture and Fix History of your WebSphere servers

### 3.4.1 Fix history

Information technology operations teams must be responsive to the latest security vulnerabilities. A patching service-level agreement (SLA) requires that an operations teams fix vulnerabilities within 30, 60 or 90 days (or faster) based on vulnerability severity.

Therefore, it is vital to be able to demonstrate the date that a vulnerability was detected, and the date that the fix was applied. WebSphere Automation provides a detailed history of fixes applied to each registered server, including information about when each issue was detected, when and how it was fixed, and how many days servers were exposed.

A record of all detected security vulnerabilities (CVEs) and the resolution status makes it easy to respond to patching SLA inquiries. WebSphere Automation keeps track of when vulnerabilities were detected, when and how they were fixed, and how many days servers were exposed.

In this section, you will review the security posture and fix history of the two traditional WebSphere servers used in the lab to demonstrate how WebSphere Automation manages the details so you can handle your patching SLAs with ease.

Now that you have applied the PH42762 iFix, let's examine the updated security posture of the 9057 WebSphere server.

1.  View the tWAS_9057_server CVE Information
    
    a. From the **Security** view in the WebSphere Automation dashboard, click on the **tWAS_9057_server** link located under the **Server** column

    ![](./lab1-media/media/imagev3-44.png)
  
    b.  Select the **Information** tab

    ![](./lab1-media/media/imagev3-45.png)

    c.  From the **Information** view, you can see the list of Installed iFixes for the 9057 server.

    ![](./lab1-media/media/imagev3-46.png)
 
    > Notice that the **Fixed date** for the iFixes related to the Log4J vulnerability show that they were installed moments ago.
    > 
    > However, there are additional iFixes that do not have a Fixed date. That is because these fixes were already applied to the server prior to it being registered with IBM WebSphere Automation.

    |                                                |                                                                                                                                                                                                                                                                                                                           |
    | ---------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | ![sign-info](./lab1-media/media/image4.png) | IBM WebSphere Automation reports ALL iFixes that have been applied, regardless of when they were applied. However. It can only determine fix dates, number of days the server was vulnerable, and vulnerability detected date for servers from the time the WebSphere server is registered with IBM WebSphere Automation. |

2.  View the tWAS_9057_server CVE Vulnerabilities details
    
    a.  From the tWS_9057_server view, select the **Vulnerabilities** tab

    ![](./lab1-media/media/imagev3-47.png)

    b. From the list of CVEs, sort by **Status**, and display the **Resolved CVEs** on top

    ![](./lab1-media/media/imagev3-48.png)
 
    The CVEs that are listed as Resolved are all the related Log4JCVEs that were fixed by applying the iFix moments ago; **CVE-2021-44228**, being the most critical CVE listed.
 
    As illustrated above, the **Vulnerabilities view** provides the details about when the vulnerability was detected in this specific server, and how many days the server has been exposed to each vulnerability.

3.  View the tWAS_9057_server Fix History details
    
    a.  From the tWS_9057_server view, select the **Vulnerabilities** tab
    
    b. Select **Fix History**, as illustrated below:

    ![](./lab1-media/media/imagev3-49.png)

4.  Notice the Fixes that have been applied

    ![](./lab1-media/media/imagev3-50.png)

    **Fix history** shows changes to the server. You can see the action that was taken, and in this case, the Fixes were “**Installed**”. Next to each Fix you can view the CVEs that were resolved by that Fix, and very importantly, the date that the Fix was applied that resolved specific vulnerabilities.
 
    Having this record of all detected security vulnerabilities (CVEs) and the resolution status makes it easy to respond to patching SLA inquiries. WebSphere Automation keeps track of when vulnerabilities were detected, when and how they were fixed, and how many days servers were exposed.

5.  You can also monitor the status of common vulnerabilities and exposures (CVEs) that affect your servers
    
    a.  Return to the **Security** view. Then select on the **CVEs** category

    ![](./lab1-media/media/imagev3-51.png)
 
    > Using this view, you can easily determine which servers are affected by specific CVEs.
    >  
    > As an example, let’s focus on CVE-2021-44228. This is one of the CVEs that was fixed in the tWAS_9057_server when you applied the iFix earlier in the lab.
    > 
    > This CVEs view helps your team quicky identify how many servers have been fixed, and how many remain exposed to specific CVEs.

    b. View the CVE-2021-44228 CVE from the current view. Notice that the registered server is patched for this CVE. There are now zero registered servers vulnerable to the CVE. 

    ![](./lab1-media/media/imagev3-52.png)


    In this section you have learned how easy IBM WebSphere Automation makes it for your Information technology operations teams to be responsive to the latest security vulnerabilities, ensuring that there is a record of patching critical CVEs in accordance with your patching service-level agreement (SLA).

    You learned how to use the automated fix deployment capabilities to easily fix vulnerable servers directly from the IBM WebSphere Automation UI. 
    
    You learned how to demonstrate the date that a vulnerability was detected, and the date that the fix was applied. WebSphere Automation provides a detailed history of fixes applied to each registered server, including information about when each issue was detected, when and how it was fixed, and how many days servers were exposed.



## Part 4: Working with WebSphere Liberty servers in IBM Automation

Registering WebSphere Liberty servers with IBM automation requires that they also register to the usage metering service as WebSphere traditional application servers. While the requirement is the same, the process in which WebSphere Liberty servers register with the metering service is quite different than WebSphere traditional. 

In Part 4 of the lab, you will perform the following task for WebSphere Liberty servers with IBM Automation. 

1.	**Register a WebSphere Liberty to IBM Automation**, using the URL, API Key, and usage Metering Certificate that you gathered in Part 2 of the lab. 

    Once registered, you will immediately see the list of all unresolved CVEs and applied iFixes for the server. 

2.	**Resolve an identified vulnerabilities in the WebSphere Liberty server**, by applying the appropriate iFix documented in the security bulletin for this vulnerability. 

    Liberty servers on the distributed platform are not vulnerable to the well-known log4J vulnerabilities. However, you will resolve a critical vulnerability in the WebSphere Liberty server. 

    Once the iFix is applied, IBM Automation immediately updates the security information for that Liberty application server, showing that the critical vulnerability has been patched.  

3.	Review the security posture and **Fix History** of the WebSphere servers.

    Once you have patched one of the Liberty servers, view the Fix History and security posture as it relates to resolved and unresolved CVEs. Here you see the Risk Level for the servers, how long servers are exposed to vulnerabilities, and when CVEs were resolved.  
   

    <br>


## 4.1 Configuring Liberty server v20.0.0.9

In this section, you configure Liberty Server version 20.0.0.9 to register to WebSphere Automation. Since Liberty servers are easily created, you will first create a new Liberty server and start it.

1. Create a new Liberty server, version 20.0.0.9, using the command below:

        /opt/IBM/WebSphere/Liberty20009/bin/server create Liberty_20009_server

    ![2009 server created](./lab1-media/media/image38.png)

	<br>

2.  Now you need to configure the server to use TLS/SSL using the provided server_tls.xml file:

        sudo \cp -f /home/ibmuser/Desktop/lab_backup/liberty20009/server_tls.xml /opt/IBM/WebSphere/Liberty20009/usr/servers/Liberty_20009_server/server.xml

    When prompted for the password for ibmuser, enter: **engageibm**

	<br>

3.  Start the Liberty server:

        /opt/IBM/WebSphere/Liberty20009/bin/server start Liberty_20009_server

    ![starting server 20009](./lab1-media/media/image39.png)

	<br>

4.  Now, you need to update the DefaultKeyStore with the server certificate to trust the server for https connection:

    The command below will import the “**cacert.pem**” file, that you saved in part 2 of the lab, into the Liberty trust store, thereby trusting the server for secure HTTPS connections from IBM Automation.

  
        keytool -import -trustcacerts -file /opt/IBM/WebSphere/cacert.pem -keystore /opt/IBM/WebSphere/Liberty20009/usr/servers/Liberty_20009_server/resources/security/key.p12 -storetype PKCS12 -storepass th1nkpassword -noprompt

    ![certificate store](./lab1-media/media/image40.png)

    <br>

5.  Configure the usage metering in the new server. This is configured in the Liberty **server.xml** file.
    
    a. Open the server.xml file using the “**gedit**” editor

        gedit /opt/IBM/WebSphere/Liberty20009/usr/servers/Liberty_20009_server/server.xml

    b.  Add the **usageMetering-1.0** feature to the existing features in the <featureManager\> element, as illustrated below:

        <feature>usageMetering-1.0</feature>
		
	
    ![](./lab1-media/media/image41.png)
	<br>

    c.  In the server.xml file, add the **usageMetering** element below:

        <usageMetering url="${metering-url}" apiKey="${api-key}" sslRef="defaultSSL"/>
    	
	![usage metering 20009](./lab1-media/media/image42.png)

	<br>

6.  **Save** and **Close** the server.xml file.
   
    <br>

7. Create a **bootstrap.properties** file for the Liberty server that contains the variable values for  **metering_url** and **api_key** that are defined in the server.xml file. Using variables will make it easier to add the configuration to the Liberty server. 

        echo "metering-url=$(cat /opt/IBM/WebSphere/metering-url.txt)" >> /opt/IBM/WebSphere/Liberty20009/usr/servers/Liberty_20009_server/bootstrap.properties


        echo "api-key=$(cat /opt/IBM/WebSphere/api-key.txt)" >> /opt/IBM/WebSphere/Liberty20009/usr/servers/Liberty_20009_server/bootstrap.properties


8.	Use the **cat** command to view the contents of the **bootstrap.properties** file to ensure it has proper values assigned to the variables

        cat /opt/IBM/WebSphere/Liberty20009/usr/servers/Liberty_20009_server/bootstrap.properties

    The **metering-url** and **api-key** variables should have values as illustrated below. 

    ![bootstrap properties](./lab1-media/media/image43.png)

    <br> 
	
9.	Restart the Liberty server so that it can be initialized with the bootstrap.properties file. 

    **Note:** Use the **--clean** option when starting the Liberty server, to clear any cached data. 

        /opt/IBM/WebSphere/Liberty20009/bin/server stop Liberty_20009_server

        /opt/IBM/WebSphere/Liberty20009/bin/server start Liberty_20009_server --clean

    ![restarted Liberty server](./lab1-media/media/image44.png)

    <br>

10.	View the Liberty server “**messages.log**” file and find the message indicating that the server was registered to the metering service.  

        tail /opt/IBM/WebSphere/Liberty20009/usr/servers/Liberty_20009_server/logs/messages.log

    ![liberty log](./lab1-media/media/image45.png)

    <br> 

    The Liberty009 server is now successfully registered with IBM Automation and should be displayed in the IBM Automation Dashboard under **Application runtimes > Security - Servers**.

    <br>

11. Go back to the browser, navigate to the **Security -> Servers** dashboard, and confirm that the Liberty_20009_server was registered in IBM Automation
    
   
    ![app runtimes servers](./lab1-media/media/image46.png)
	
	<br>


    |         |           |  
    | ------------- |:-------------|
    | ![](./lab1-media/media/image47.png?cropResize=50,50)   | <strong>IMPORTANT:</strong> <br><br> If the IBM Automation UI does not automatically detect the Liberty server that you registered, then it is extremely likely that the server.xml file or the bootstrap.properties file has not been configured correctly. <br><br> 1. Review the <strong>server.xml</strong> configuration. <br> &nbsp;&nbsp;&nbsp;&nbsp;- Ensure the <strong>usageMetering-1.0</strong> feature is included in the list of features. <br><br> 2. Review the **bootstrap.properties** file<br> &nbsp;&nbsp;&nbsp;&nbsp;- Ensure the usageMetering <strong>url</strong> is CORRECT. No additional characters or spaces. <br> &nbsp;&nbsp;&nbsp;&nbsp;- Ensure the usageMetering <strong>apiKey</strong> is CORRECT. No additional characters or spaces. 


    If the Liberty server was successfully registered, it is displayed in the Application Runtimes in IBM automation UI.


    |         |           |  
    | ------------- |:-------------|
    | ![](./lab1-media/media/image4.png?cropResize=50,50)   | <strong>TIP:</strong> <br><br><strong>Note:</strong> New vulnerabilities are discovered constantly, so the number of CVEs discovered may be different than illustrated.

    The dashboard shows that this server is vulnerable for some unresolved CVEs.

    <br>

12.	Click on the “**+2 more**” under the Unresolved CVEs for the Liberty_20009_server, to see the list of the unresolved CVEs, including the CVE-2020-10693.

    Note that the +2 could be different if additional CVEs have been discovered since the time of this writing. 

    ![app runtimes servers](./lab1-media/media/image48.png)

    <br>

    a. The list of Unresolved CVEs for Liberty version 20.0.0.9 should include CVE-2020-10693

      ![app runtimes servers](./lab1-media/media/image49.png)

    <br>

13. Check your email that you registered with IBM automation for a notification of new vulnerability. 

      ![email](./lab1-media/media/image50.png)

      <br>


## 4.2 Review CVE-2020-10693 to understand the remediation options for the vulnerability

In this section, you will review the details of the critical **CVE-2020-10693** CVE and determine the appropriate remediation options to resolve the CVE.

In this lab, you will apply the iFix that is documented in the IBM security bulletin to resolve the vulnerability in the Liberty 20009 server, as reported in **CVE-2020-10693**.

1.  View the CVE information for CVE-2020-10963
    
    a. Click the **Liberty_2009_server** link under the "Server" column to see CVE and Fix History for this Liberty server

    ![](./lab1-media/media/imagev3-72.png)

    b. Click on CVE-2020-10693 CVE located in the **Vulnerabilities > CVEs** view

    ![](./lab1-media/media/imagev3-73.png)

    c.  The **CVE information page** for CVE-2020-10693 is displayed.

    ![](./lab1-media/media/imagev3-74.png)

    From the CVE Information page that is displayed, you can see important details about this CVE.

    <u>Here you can see the following details:</u>

    - Risk Level “**Medium (5.3)”**

    - Date the vulnerability was detected in the “**registered**” server

    - Number of days the Liberty server has been “**exposed**” to the vulnerability

    - List of affected servers that are registered with IBM WebSphere Automation.

    - Link to the IBM security bulletin for the CVE

    > **Note:** The **Days Exposed** and **date detected** are expressed from the point of time that the server is registered with IBM WebSphere Automation.

2.  Click on the **View** Link to view the IBM security bulletin for this CVE.

    ![](./lab1-media/media/imagev3-75.png)
 
    The security bulletin is displayed in a new browser tab.

3.  From the Web Browser, review the security bulletin to learn about the vulnerability, versions of WebSphere affected, and options for remediating the vulnerability.

    ![](./lab1-media/media/imagev3-76.png)

    As you see, IBM WebSphere Automation makes it easy for you to get to the related security bulletins directly from the CVE information within the WebSphere Automation dashboard so you can have all the details and remediation options at your fingertips

    > **Note:** The IBM Security Bulletin associated with this vulnerability documents the options for remediating CVE-2020-10693

    ![](./lab1-media/media/imagev3-77.png)

    ![](./lab1-media/media/imagev3-78.png)

    In the next section of the lab, you will apply the Interim Fix **PH29942** to remediate the unresolved CVE.

4.  When you finish reviewing the security bulletin, return to the Browser tab that contains the IBM WebSphere Automation dashboard. Then return to the “**Security**” view

    ![](./lab1-media/media/imagev3-79.png)



## 4.3 Update Liberty server v20.0.0.9 to fix the vulnerability

Automated installation of WebSphere and Liberty fixpaks and iFixes was introduced into IBM WebSphere Automation in version 1.4

You can automatically deploy fixes to servers in your managed server inventory through the WebSphere Automation UI. You must have a user profile with at least the **Modify WebSphere inventory** permission, and the servers that you want to fix must be registered and configured for fix installation with WebSphere Automation.

In this section, you will leverage the built-in capabilities in IBM WebSphere Automation to automatically prepare and install the recommended iFix to resolve one vulnerability in the Liberty Server
v20.0.0.9. 

There are a couple of additional components now included in the product to support this use case. 

  - **Fix manager**
    
    The WebSphere Automation fix manager uses credentials that you provide to access IBM Fix Central to request fixes. Fixes are fetched and stored in the file storage defined for the websphereSecure custom resource. The fix manager manages the storage space according to frequency of use, deleting older fixes to make room for more recently requested fixes.

  - **Installation manager**
  
    The WebSphere Automation installation manager communicates with the registered server using the administrator privileges that you provide. When you initiate the installation of a fix, the installation manager ensures that the target server has sufficient space for the fix, transfers the fix to the target server, installs the fix, and creates a log file of the steps taken. If you request a backup of the server environment as part of the fix installation, the installation manager checks for sufficient disk space on the server, and creates an archive of the Installation Manager, Installation Manager data, and WebSphere Application Server or WebSphere Application Server Liberty server installation directories.

### 4.3.1 Navigate to the Server and unresolved CVE to fix

OK, let's apply the recommended iFix to resolve the **CVE-2020-10693** vulnerability in the Liberty server. 

1.  Notice in the IBM Automation console that Liberty20009_server is vulnerable to **CVE-2020-10693**.

    ![](./lab1-media/media/image99.png)

    
   
2.	Expand the list of unresolved CVEs. Notice CVE-2020-10693 is listed.  

    ![](./lab1-media/media/image98.png)

   
    **Note:** The IBM Security Bulletin associated with this vulnerability documents the options for remediating **CVE-2020-10693**  

    ![](./lab1-media/media/image100.png)

    
    Now, you will fix it by applying the recommended iFix. (PH29942)

    <br>

3. Click on **Liberty_20009_server** to view the list of CVEs / vulnerabilities
 
     ![](./lab1-media/media/image102.png)
    

    The displayed list shows both "resolved" and "unresolved" CVEs for the selected server. It also shows the "Risk Level" to identify the severity of the vulnerabilities, and the number of days the server has been exposed to the vulnerability. 

    ![](./lab1-media/media/image101.png)
 
4. Click on CVE-2020-10693 to view the CVE information and affected servers

    ![](./lab1-media/media/image103.png)

    On the CVE Information page for CVE-2020-10693, a list of **Affected Servers** is displayed along with the option to **Prepare fix**. 

    **DO NOT prepare fix yet.** You will do that in the following steps. 

    ![](./lab1-media/media/image104.png)
    


### 4.3.2 Prepare Fix 

The **Fix Deployment** capability of WebSphere Automation delivers automation that determines which APARs and interim fixes resolve a specific vulnerability, and enables one-click download and deploy of fixes. 

Selection of the wanted iFix or published fix pack is exposed through the **Prepare fix** dialog, which can be found in the CVE details view. Simply select the target server to patch and the dialog shows all the affected servers known to WebSphere Automation, and lets you select the wanted fix.

After you select the fix, WebSphere Automation provides two options: 

  - **Fetch fix:** 

    If installation should be deferred to a later time, then the **Fetch fix** option causes the fix to be downloaded and stored for later use. 
  
    This can be preferable for fix packs because those are significantly larger than interim fixes, especially for traditional WebSphere / Liberty fix packs.
  
  - **Fetch then install fix:** 

    The **Fetch then install fix** option automatically downloads the fix and then installs it after the download is complete. 
  
    Previously fetched fixes are stored within WebSphere Automation for immediate reuse. 

    <br/>  

    > Now you will download and install the recommended iFix for the CVE into the Liberty_20009_server    

1. Download the recommended iFix to resolve the CVE
 
    Next to each affected server, there is an option to "**Prepare Fix**". 
    
    a. Click the **Prepare fix** link next to **Liberty_20009_server**  

    ![](./lab1-media/media/image105.png)

    b. From the **Select fix** page, select the **PH29942** iFix. Then click the **Fetch fix** button. 

    ![](./lab1-media/media/image106.png)


    This action will cause WebSphere Automation to fetch the fix from IBM Fix Central, using the credentials that you configured at the beginning of the lab. 

    The **Status** field automatically updates as the action progresses. 

    ![](./lab1-media/media/image107.png)

    When the fix has been successfully fetched from IBM Fix Central, the status will change to **Ready to Install**.

    ![](./lab1-media/media/image108.png)


2. Install the fix to resolve the CVE

    Once the fix has been fetched, the action to **Install the fix** is made available next to the affected server.

    a. Click on the **Install fix** action next to the Liberty_20009_server    

    ![](./lab1-media/media/image109.png)

    b. When prompted to confirm the installation, click **Proceed**

    ![](./lab1-media/media/image110.png)


 
	|         |           |  
    | ------------- |:-------------|
    | ![](./lab1-media/media/image4.png?cropResize=100,100)   | <strong>Alert:</strong> <br>In this lab environment, when the Install Fix action is initiated, a pop-up error dialog may be displayed that may be ignored!<br/> <br/>![](./lab1-media/media/image112.png) <br/> If the "Status" field has not changed to "Failed", let the installation proceed to completion. 

    
    c. The installation may take 5 or 6 minutes to complete. Wait until you see the status change to "**Installation complete**"

    ![](./lab1-media/media/image113.png)


    > **TROUBLESHOOTING TIP**
    > 
    > If the installation fails, click on the **ID** of the action, and view the runbook.log output file. 
    >
    >  Common errors we have seen reported in the log is the ssh key and/or the wsa-ansible secret not properly configured in the environment.  
    >
    > ![](./lab1-media/media/image114.png)
    >
    > ![](./lab1-media/media/image115.png)

   

**Congratulations!** You have successfully applied the iFix to resolve the critical CVE on the Liberty_20009_server. 

In the next section of the lab, you will review the updated security posture and fix history of the Liberty server. 

<br>


## 4.4 View the security posture and Fix History of your Liberty server

### 4.4.1 Fix history

Information technology operations teams must be responsive to the latest security vulnerabilities. A patching service-level agreement (SLA) requires that an operations teams fix vulnerabilities within 30, 60 or 90 days (or faster) based on vulnerability severity.

Therefore, it is vital to be able to demonstrate the date that a vulnerability was detected, and the date that the fix was applied. WebSphere Automation provides a detailed history of fixes applied to each registered server, including information about when each issue was detected, when and how it was fixed, and how many days servers were exposed.

A record of all detected security vulnerabilities (CVEs) and the resolution status makes it easy to respond to patching SLA inquiries. WebSphere Automation keeps track of when vulnerabilities were detected, when and how they were fixed, and how many days servers were exposed.

In this section, you will review the security posture and fix history of the two traditional WebSphere servers used in the lab to demonstrate how WebSphere Automation manages the details so you can handle your patching SLAs with ease.

Now that you have applied the PH29942 iFix, let’s examine the updated security posture of the Liberty server.

1.  View the Liberty_20009_server CVE Information
    
    a.  From the **Security** view in the WebSphere Automation dashboard, click on the **Liberty_20009_server** link located under the **Server** column

    ![](./lab1-media/media/image116.png)

    b.  Select the **Information** tab

    ![](./lab1-media/media/imagev3-84.png)

    c. From the **Information** view, you can see the list of Installed iFixes for the Liberty server.

    ![](./lab1-media/media/image117.png)
 
    Notice that the **Fixed date** for the iFixes related to the vulnerability show that it was fixed moments ago.

    <table>
    <tbody>
    <tr class="odd">
    <td><img src="./lab1-media/media/image4.png" style="width:1.60417in;height:1.60417in" alt="sign-info" /></td>
    <td><p><strong>Information:</strong></p>
    <p>IBM WebSphere Automation reports ALL iFixes that have been applied, regardless of when they were applied. However. It can only determine fix dates, number of days the server was vulnerable, and vulnerability detected date for servers from the time the WebSphere server is registered with IBM WebSphere Automation.</p>
    <p>If the servers already had iFixes installed prior to being registered to IBM WebSphere Automation, they would be listed, but the installed date in not known and will be blank.</p></td> 
    </tr>
    </tbody>
    </table>

2.  View the Liberty_20009_server CVE Vulnerabilities details
    
    a.  From the Liberty_20009_server view, select the **Vulnerabilities** tab

    ![](./lab1-media/media/imagev3-86.png)

    b.  From the list of CVEs, sort by **Status**, and display the **Resolved CVEs** on top. The **CVE-2020-10693** is now **Resolved**.

    ![](./lab1-media/media/image118.png)
 
    As illustrated above, the **Vulnerabilities view** provides the details about when the vulnerability was detected in this specific server, and how many days the server has been exposed to each vulnerability.

3.  View the Liberty_20009_server Fix History details
    
    a.  From the Liberty_20009_server view, select the **Vulnerabilities** tab
    
    b.  Select **Fix History**, as illustrated below:

    ![](./lab1-media/media/imagev3-88.png)

4.  Notice there is now a record that shows that the iFix has been installed, and the date te server was patched.

    ![](./lab1-media/media/image119.png)
 
    **Fix history** shows changes to the server. You can see the action that was taken, and in this case, the Fixes were “**Installed**”. Next to each Fix you can view the CVEs that were resolved by that Fix, and very importantly, the date that the Fix was applied that resolved specific vulnerabilities.
 
    Having this record of all detected security vulnerabilities (CVEs) and the resolution status makes it easy to respond to patching SLA inquiries. WebSphere Automation keeps track of when vulnerabilities were detected, when and how they were fixed, and how many days servers were exposed.

5.  You can also monitor the status of common vulnerabilities and exposures (CVEs) that affect your servers
    
    a.  Return to the **Security** view. Then select on the **CVEs** category

    ![](./lab1-media/media/image120.png)
 
    Using this view, you can easily determine which servers are affected by specific CVEs.
 
    As an example, let’s focus on CVE-2020-10693. This is the CVEs that was fixed in the Liberty when you applied the iFix moments ago.
 
    This **CVEs view** helps your team quicky identify how many servers have been fixed, and how many remain exposed to specific CVEs.

    b.  Locate and view the CVE-2020-10693 CVE from the current view. Sort the list of CVEs by Fixed Servers, which will list the CVEs that have been applied to the Liberty server at the top of the list.

    ![](./lab1-media/media/image121.png)

6.  Identify the Liberty servers that this CVE has been resolved, and potentially still unresolved.
    
    a. From the CVE view, filter by CVE: **CVE-2020-10693**

    ![](./lab1-media/media/image122.png)

    a.  Click on the **CVE-2020-10693** link under the **CVE** column

    ![](./lab1-media/media/image123.png)

    b.  Note the **Liberty_20009_server** Vulnerability Status is “**Resolved**”.

    ![](./lab1-media/media/image124.png)

    In this section you have learned how easy IBM Automation makes it for your Information technology operations teams to be responsive to the latest security vulnerabilities, ensuring that there is a record of patching critical CVEs in accordance with your patching service-level agreement (SLA.

    You learned how to demonstrate when the date that a vulnerability was detected, and the date that the fix was applied. WebSphere Automation provides a detailed history of fixes applied to each registered server, including information about when each issue was detected, when and how it was fixed, and how many days servers were exposed.



## Summary

Congratulations! You have completed the WebSphere Automation lab.

With automated tooling and insights, IBM WebSphere Automation enables
teams to modernize and secure IT estates, adapt and respond to incidents
efficiently, and optimize WebSphere operations. WebSphere system
operators and administrators can reduce the cost, effort, and risk of
addressing vulnerabilities, automate critical activities, and preserve
uptime with early detection, notification, and remediation of incidents.

IBM WebSphere Automation helps teams remove manual toil to work less on
maintenance tasks and more on strategic activities, while unlocking new
value, extending the life, and increasing ROI of WebSphere investments.

IBM WebSphere Automation is part of IBM Automation, a set of shared
automation services that help you get insight into how your processes
run, visualize hotspots and bottlenecks, and use financial impact
information to prioritize which issues to address first.

To learn more about IBM WebSphere Automation, visit
[<span class="underline">ibm.com/cloud/websphere-automation</span>](http://ibm.com/cloud/websphere-automation).



# Appendix 1: Manual installation of WebSphere iFix to resolve CVE

## Update tWAS server v9.0.5.7 to fix the LOG4J vulnerability

In this section, you will apply an iFix to the traditional WebSphere 9.0.5.7 server to remove the Log4J vulnerability reported as **CVE-2021-44228**.

1.  Notice in the IBM Automation console that tWAS 9.0.5.7 is vulnerable to **CVE-2021-44228**.

    ![CVE 2021 44228](./lab1-media/media/image30.png)

    Now, you will fix it by applying the appropriate iFix.
	
    <br>

2.	Expand the list of unresolved CVEs. IBM Automation also detected the additional unresolved CVEs related to the Log4J vulnerability, which are highlighted below and documented in the security bulletin. 

    ![additional log4j cves](./lab1-media/media/image31.png)

   

    **Note:** The IBM Security Bulletin associated with this vulnerability documents the options for remediating **CVE-2021-44228** and related CVEs (**CVE-2021-4104** and **CVE-2021-45046**) 

    ![security bulletin info](./lab1-media/media/image32.png)

    <br>

    You can read the security bulletin here. You may be prompted to login using your IBM ID to access the content: 

    [https://www.ibm.com/support/pages/security-bulletin-multiple-vulnerabilities-apache-log4j-affect-ibm-websphere-application-server-and-ibm-websphere-application-server-liberty-cve-2021-4104-cve-2021-45046](https://www.ibm.com/support/pages/security-bulletin-multiple-vulnerabilities-apache-log4j-affect-ibm-websphere-application-server-and-ibm-websphere-application-server-liberty-cve-2021-4104-cve-2021-45046)


    Now, you will fix it by applying the appropriate iFix. (PH42762)

    <br>

3.  First, stop the server, wait until it has stopped:

        /opt/IBM/WebSphere/AppServer9057/bin/stopServer.sh tWAS_9057_server

    ![](./lab1-media/media/image33.png)
	
    <br>
	
4.  Run the following script to install the iFix:

        sudo /iFix/PH42762-LOG4J/imcl_ifix_install.sh 9057

    When prompted for the password for the ibmadmin user, enter: **engageibm**

    ![](./lab1-media/media/image34.png)
	
	<br>

5.  Start the server once the iFix installation has completed:

        /opt/IBM/WebSphere/AppServer9057/bin/startServer.sh tWAS_9057_server

6.  Go back to the WebSphere Automation dashboard. Notice that the CVE-2021-44228, CVE-2021-4104, and CVE-2021-45046 were removed from the unresolved CVEs list for the tWAS 9.0.5.7 server.

    ![cves resolved](./lab1-media/media/image35.png)

    <br>

7.	Also, you can see on the right under “Applied iFixes” the PH42762 fix is now listed as applied. 


    ![ifix applied](./lab1-media/media/image36.png)


    <br>

## Update tWAS server v9.0.5.7 to introduce the vulnerability back (optional)

You can remove the iFix that was applied to confirm that the CVE-2021-44228 shows up in the unresolved CVE list.

1.  Stop the server:

        /opt/IBM/WebSphere/AppServer9057/bin/stopServer.sh tWAS_9057_server

2.  Uninstall the iFix:

        sudo /iFix/PH42762-LOG4J/imcl_ifix_uninstall.sh 9057

    When prompted for the password for the ibmadmin user, enter: **engageibm**

    <br>

3.  Start the server:

        /opt/IBM/WebSphere/AppServer9057/bin/startServer.sh tWAS_9057_server

4. Go back to the WebSphere Automation Dashboard on your browser. Notice that CVE-2021-44228, CVE-2021-4104, and CVE-2021-45046 are once again listed as unresolved CVEs for the tWAS 9.0.5.7 server.

    ![CVE 2021 44228](./lab1-media/media/image37.png)

  
# Appendix 2: Manual installation of Liberty iFix to resolve CVE

## Update Liberty server v20.0.0.9 to fix the vulnerability

In this section, you will fix one vulnerability in our Liberty Server
v20.0.0.9 using two different approaches. First, you fix by only
updating the configuration. Later, you apply an iFix to solve the
vulnerability.

### Updating configurations

Liberty 20.0.0.9 is impacted by CVE-2020-10693. This is because it
configures the **beanValidation-2.0** feature. See
[<span class="underline">CVE-2020-10693</span>](https://www.ibm.com/support/pages/node/6348216)
for more info.

We should resolve this by applying the iFix **PH29942** as documented in
the CVE-2020-10693. However, for the lab, we can also do a quick test by
unconfiguring this feature, to illustrate that IBM Automation does
interrogate the Liberty Server configuration, to determine if a Liberty
server is impacted by a CVE that is the result of a specific
configuration feature. 

This is valuable because IBM Automation can
pinpoint specific servers that are impacted not ONLY based on the
version of server, but also the specific configuration that is impacted
by the CVE.

1.  Edit the server.xml, using the command below:

        gedit /opt/IBM/WebSphere/Liberty20009/usr/servers/Liberty_20009_server/server.xml

2.  Comment out the **beanValidation-2.0** feature:

        <!-- <feature>beanValidation-2.0</feature> -->

    ![](./lab1-media/media/image51.png)
	
	<br>

3.  **Save** and **close** the *server.xml* file.

    <br>

4.  Back to your browser, check that the Liberty 20.0.0.9 server does **NOT** show the **CVE-2020-10693** vulnerability. The update is picked up automatically.

    ![no CVE 2020 10693](./lab1-media/media/image52.png)
 
    Great, you removed the vulnerability by updating the configuration.


### Resolve the issue By Applying an iFix

However, instead of removing the beanValidationFeature-2.0, the correct
process is to apply the appropriate iFix to get rid of the
vulnerability.

1.  First, add back the beanValidation feature:

        gedit /opt/IBM/WebSphere/Liberty20009/usr/servers/Liberty_20009_server/server.xml

2.  Uncomment the **beanValidation-2.0** feature:

        <feature>beanValidation-2.0</feature> 

    ![](./lab1-media/media/image53.png)
	
	<br>

3.  **Save** and **Close** the server.xml file.

    <br>

4.  Back to the browser, make sure the vulnerability shows up again.

    ![cve back](./lab1-media/media/image54.png)
	
	<br>

    Next, resolve the CVE by applying the recommended iFix (PH29942) documented in the security bulletin for this vulnerability. 

    <br>

5.  You need to **stop** the server before the iFix can be applied. Return to the terminal window and run the command below to stop the Liberty2009 server.

        /opt/IBM/WebSphere/Liberty20009/bin/server stop Liberty_20009_server

6. Now, apply the iFix, by running the following command.

    **Note:** We have already downloaded the iFixes used for this lab. They are stored in **/iFix** directory on the **STUDENT** VM.
    
        sudo /iFix/PH29942/imcl_ifix_install.sh 20009

    When prompted for the password for ibmuser, enter: **engageibm**

     ![apply fix](./lab1-media/media/image55.png)
	
	<br>

7. Great, the iFix was applied. Now, start the server again:

        /opt/IBM/WebSphere/Liberty20009/bin/server start Liberty_20009_server

8. Return to the **WebSphere Automation Dashboard.** You should notice that the **CVE-2020-10693** was removed from the Liberty_20009_server.

    ![no CVE 2020 10693](./lab1-media/media/image56.png)

    ![no CVE 2020 10693a](./lab1-media/media/image57.png)

    <br>

9.	Note that the IBM Automation dashboard shows the iFix PH29942 has been applied to the Liberty_20009_server.

    ![ifixes appli](./lab1-media/media/image58.png)

   
