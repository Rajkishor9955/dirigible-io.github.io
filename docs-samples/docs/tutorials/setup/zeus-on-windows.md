---
title: Zeus on Kubernetes with Windows OS
---

Zeus on Kubernetes with Windows OS
===

This tutorial was performed on a PC running Windows 10 Enterprise OS.

### Prerequisites

* Have [VirtualBox 5.2.12 platform packages](https://download.virtualbox.org/virtualbox/5.2.12/VirtualBox-5.2.12-122591-Win.exe) installed
* .NET Framework 4+ (the installation will attempt to install .NET 4.0 if you do not have it installed)
* Enabled VT-x or AMD-v virtualization (use the Performance tab for the CPU in the Task Manager to verify it)

### Install Kubernetes command-line tool

* Install **Chocolatey** 

 * Run the following command:

> @"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"

If you don't see any errors, you are ready to use Chocolatey! 

* To ensure that Chocolatey is successfully installed, type **choco** or **choco -?**.

For more information see https://chocolatey.org/install.

* Install the Kubernetes command-line tool **kubectl** with Chocolatey 

 * Execute the command:

> choco install kubernetes-cli

 * To verify that the version you’ve installed is up-to-date, run 

> kubectl version

 * Configure kubectl to use a remote Kubernetes cluster:

> cd C:\users\yourusername (Or wherever your %HOME% directory is)  
mkdir .kube cd .kube New-Item config -type file

* Edit the config file with a text editor of your choice.

 * Check that kubectl is properly configured by getting the cluster state:

> kubectl cluster-info

### Install Minikube

* Install **Minikube v0.26.1**

Download the [minikube-installer.exe](https://github.com/kubernetes/minikube/releases/download/v0.26.1/minikube-installer.exe) file, and execute the installer. This will automatically add minikube.exe to your path.

### Additional Steps 

#### Install Docker

* Run CMD as Administrator

* Navigate to Chocolatey **root** folder

3. Execute: 

> choco install docker

* Confirm with **y**

#### Build the image

Build an image without uploading it:

* Set the environment variables with 

> @FOR /f "tokens=*" %i IN ('minikube docker-env') DO @%i

* Clone the Zeus packaging project using Git Bash by executing:

> git clone https://github.com/dirigiblelabs/zeus-v3-package.git

* Build the image with the Docker daemon of Minikube:

> cd zeus-v3-package/zeus

> mvn clean install

> docker build -t zeus .

* Set the image in the pod spec like the build tag: **zeus**
* Set the **imagePullPolicy** to **Never**, otherwise Kubernetes will try to download the image

> Important note: You have to run eval ** $(minikube docker-env)** on each terminal you want to use, since it only sets the environment variables for the current shell session.
