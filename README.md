# AFFiNE: Deployment file for Jelastic
AFFiNE deployment manifest file for Jelastic

##  1. <a name='TableofContents'></a>Table of Contents
<!-- vscode-markdown-toc -->
* 1. [Table of Contents](#TableofContents)
* 2. [Introduction](#Introduction)
* 3. [Warning](#Warning)
* 4. [Stack](#Stack)
* 5. [Deployment](#Deployment)
* 6. [Add Basic Authentication](#AddBasicAuthentication)
* 7. [License](#License)
* 8. [Acknowledgments](#Acknowledgments)

<!-- vscode-markdown-toc-config
	numbering=true
	autoSave=true
	/vscode-markdown-toc-config -->
<!-- /vscode-markdown-toc -->

##  2. <a name='Introduction'></a>Introduction

This repository contains the manifest file for deploying [AFFiNE](https://affine.pro/) on Jelastic cloud platform.

##  3. <a name='Warning'></a>Warning

By default, AFFiNE does not offer an authentication mechanism. This means that anyone with the URL can access the AFFiNE instance. It is recommended to use a reverse proxy with authentication in front of AFFiNE.

##  4. <a name='Stack'></a>Stack

The `manifest.jps` describes the deployment of AFFiNE on Jelastic. It is composed of 2 parts:

- Load Balancer (NGINX) : this is the entry point of the application. It is used to distribute the traffic to the AFFiNE instance.

- AFFiNE : this is the AFFiNE instance. It is composed of the latest dokcer version `affine:nightly-server-latest`

##  5. <a name='Deployment'></a>Deployment

To deploy AFFiNE on Jelastic instance, you need to import the `manifest.jps` file in the Jelastic dashboard.

Use this link to import the manifest:

```
https://raw.githubusercontent.com/yoanbernabeu/AFFiNE_Jelastic/main/manifest.jps
```

##  6. <a name='AddBasicAuthentication'></a>Add Basic Authentication

To add basic authentication to the AFFiNE instance, you need to add a [HTTP Basic Authentication](https://docs.nginx.com/nginx/admin-guide/security-controls/configuring-http-basic-authentication/) in the Load Balancer.

* Open a terminal in your computer

* Create a file `htpasswd` with the following command:

```
htpasswd -c ~/htpasswd <username>
```

* Enter the password when prompted

* Copy the file `htpasswd` to the Load Balancer in a `/var/lib/nginx/htpasswd`

* Edit the file `/etc/nginx/nginx-jelastic.conf` and add the following lines:

```conf
// ...

server {
    listen *:80;
    listen [::]:80;
    server_name  _;
				
    auth_basic           "AFFiNE";
    auth_basic_user_file /var/lib/nginx/htpasswd; 				

// ...
```

* Restart the Load Balancer

##  7. <a name='License'></a>License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details

##  8. <a name='Acknowledgments'></a>Acknowledgments

* [Jelastic](https://jelastic.com/) for the deployment platform
* [AFFiNE](https://affine.pro/) for the software
* [Hidora Cloud Provider](https://www.hidora.io/) for the cloud infrastructure