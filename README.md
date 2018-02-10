# Flask app using Twitter Bootstrap

## Content

* [Run it](#run-it)
* [Deploying a containerized web application on Google Cloud](#deploying-a-containerized-web-application-on-google-cloud)
   * [Dockerize your webapp](#dockerize-your-webapp)
      * [Push your image to DockerHub](#push-your-image-to-dockerhub)
      * [Push your image to Google Image Registry](#push-your-image-to-google-image-registry)
   * [Install Google Cloud SDK](#install-google-cloud-sdk)
      * [Google Cloud comes with "components"](#google-cloud-comes-with-components)
      * [Install Kubernetes components](#install-kubernetes-components)
      * [From Google Cloud Console](#from-google-cloud-console)
      * [Get info on the project](#get-info-on-the-project)
   * [Create your cloud cluster with Google Cloud SDK](#create-your-cloud-cluster-with-google-cloud-sdk)
      * [Run our image as an app on the cluster](#run-our-image-as-an-app-on-the-cluster)
      * [Allow the cluster to be accessible from the outside](#allow-the-cluster-to-be-accessible-from-the-outside)
   * [Manage the Domain Name of the webapp](#manage-the-domain-name-of-the-webapp)
      * [Configuring Domain Names with Static IP Addresses](#configuring-domain-names-with-static-ip-addresses)
   * [Deploy a new version of the webapp:](#deploy-a-new-version-of-the-webapp)




## Test it first!

To test the app, you can either use Docker, or run it directly as a local Flask app.

### Using Docker

* Install the [Docker engine](https://docs.docker.com/install/)
* Run the webapp in this simple command:

```shell
docker run -d -p 5000:5000 dataforgood/site
```

Open it in your browser: http://localhost:5000/

### Without Docker

```shell
pip install -r requirements.txt
python app.py
```



## Building your responsive Flask app

This [Youtube tutorial](https://www.youtube.com/watch?v=3NsEGaCIT38) will help you build your "one-page" Flask app as a responsive webapp thanks to Twitter Bootstrap.

Simply follow the steps in the video! 


## Dockerize your webapp

To Dockerize your webapp, you will need to create a 'recipe' (`Dockerfile`) which explains all steps necessary to rebuild the entire app from the choice of the operating system.

This nice [tutorial](https://medium.com/@ikod/dockerize-simple-python-flask-app-62461efbe58e) describes the steps to take to dockerize your existing webapp.

Once your `Dockerfile` is created, you can build and test your image with:

```shell
$ docker build -t dataforgood/site:latest .
$ docker run -d -p 5000:5000 dataforgood/site
```

Open the app in your browser at: http://localhost:5000/





## Deploying your containerized webapp on Google Cloud

[https://cloud.google.com/kubernetes-engine/docs/tutorials/hello-app](https://cloud.google.com/kubernetes-engine/docs/tutorials/hello-app)


### Install Google Cloud SDK

Google provides the steps for almost every OS: [https://cloud.google.com/sdk/docs/quickstarts](https://cloud.google.com/sdk/docs/quickstarts)

Check your installation with:

    $ gcloud

which should give you some help messages.


### Push your Docker image on a cloud registry

#### Push your image to DockerHub

(https://hub.docker.com/u/dataforgood/)

     $ docker tag 71df3012md... dataforgood/site
     $ docker push dataforgood/site

#### Push your image to Google Image Registry

     $ gcloud docker push gcr.io/dataforgood/site


#### Google Cloud comes with "components"

     $ gcloud components list

#### Install a new component: Kubernetes 

     $ gcloud components install kubectl


#### Create your cloud project

From Google Cloud Console ([https://console.cloud.google.com](https://console.cloud.google.com/?pli=1)):

* create a new project for your project, and note your project ID
* on your machine, create a variable with this project ID

```shell
$ export PROJECT_ID=demo-kubernetes-webapp-124242
```

* We will need the Google Container Engine API to make use of kubernetes.

So we enable it: on Cloud platform, sidebar, API Manager, click on button "ENABLE API", search for "Google Container Engine API"

* Initialize a Google Cloud console associate to your Google Cloud platform

```shell
$ gcloud init
```

You are logged in as: [patrick@dataforgood.no].

```shell
Pick cloud project to use: 
 [1] demobookshelf-192122
 [2] website-192121
 [3] weighty-triode-185408
 [4] Create a new project
Please enter numeric choice or text value (must exactly match list 
item):  2

Your current project has been set to: [website-192121].

Not setting default zone/region (this feature makes it easier to use
[gcloud compute] by setting an appropriate default value for the
--zone and --region flag).
See https://cloud.google.com/compute/docs/gcloud-compute section on how to set
default compute region and zone manually. If you would like [gcloud init] to be
able to do this for you the next time you run it, make sure the
Compute Engine API is enabled for your project on the
https://console.developers.google.com/apis page.

Created a default .boto configuration file at [/home/patechoc/.boto]. See this file and
[https://cloud.google.com/storage/docs/gsutil/commands/config] for more
information about configuring Google Cloud Storage.
Your Google Cloud SDK is configured and ready to use!
```

* Commands that require authentication will use patrick@dataforgood.no by default
* Commands will reference project `website-192121` by default

Run `gcloud help config` to learn how to change individual settings

This gcloud configuration is called [default]. You can create additional configurations if you work with multiple accounts and/or projects.

Run `gcloud topic configurations` to learn more.

Some things to try next:

* Run `gcloud --help` to see the Cloud Platform services you can interact with. And run `gcloud help COMMAND` to get help on any gcloud command.
* Run `gcloud topic -h` to learn about advanced features of the SDK like arg files and output formatting

#### Get info on the project

```shell
$ gcloud info
Google Cloud SDK [184.0.0]

Platform: [Linux, x86_64] ('Linux', 'jarvis', '4.10.0-42-generic', '#46-Ubuntu SMP Mon Dec 4 14:38:01 UTC 2017', 'x86_64', 'x86_64')
Python Version: [2.7.13 (default, Nov 23 2017, 15:37:09)  [GCC 6.3.0 20170406]]
Python Location: [/usr/bin/python2]
Site Packages: [Disabled]

Installation Root: [/usr/lib/google-cloud-sdk]
Installed Components:
  core: [2018.01.05]
  beta: [2018.01.05]
  gsutil: [4.28]
  bq: [2.0.28]
  alpha: [2018.01.05]
System PATH: [/home/patechoc/anaconda3/bin:/home/patechoc/anaconda3/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin]
Python PATH: [/usr/bin/../lib/google-cloud-sdk/lib/third_party:/usr/lib/google-cloud-sdk/lib:/home/patechoc/Documents/CODE-DEV/quiztools/quiztools:/home/patechoc/MLSuite//aiaml:/home/patechoc/Download/MLSuite/aiaml::/usr/lib/python2.7:/usr/lib/python2.7/plat-x86_64-linux-gnu:/usr/lib/python2.7/lib-tk:/usr/lib/python2.7/lib-old:/usr/lib/python2.7/lib-dynload]
Cloud SDK on PATH: [False]
Kubectl on PATH: [/usr/bin/kubectl]

Installation Properties: [/usr/lib/google-cloud-sdk/properties]
User Config Directory: [/home/patechoc/.config/gcloud]
Active Configuration Name: [default]
Active Configuration Path: [/home/patechoc/.config/gcloud/configurations/config_default]

Account: [patrick@dataforgood.no]
Project: [website-192121]

Current Properties:
  [core]
    project: [website-192121]
    account: [patrick@dataforgood.no]
    disable_usage_reporting: [True]

Logs Directory: [/home/patechoc/.config/gcloud/logs]
Last Log File: [/home/patechoc/.config/gcloud/logs/2018.01.21/22.40.12.932912.log]

git: [git version 2.11.0]
ssh: [OpenSSH_7.4p1 Ubuntu-10, OpenSSL 1.0.2g  1 Mar 2016]
```



### Create your cloud cluster with Google Cloud SDK

```shell
$ gcloud config configurations list
NAME     IS_ACTIVE  ACCOUNT                 PROJECT         DEFAULT_ZONE  DEFAULT_REGION
default  True       patrick@dataforgood.no  website-192121

$ gcloud config configurations activate default
$ gcloud compute project-info add-metadata \ --metadata google-compute-default-region=europe-west1,google-compute-default-zone=europe-west1-b
$ gcloud container clusters create d4gcluster  # NOT ENOUGH
$ gcloud container clusters create d4gcluster --zone europe-west1-b

WARNING: Starting in Kubernetes v1.10, new clusters will no longer get compute-rw and storage-ro scopes added to what is specified in --scopes (though the latter will remain included in the default --scopes). To use these scopes, add them explicitly to --scopes. To use the new behavior, set container/new_scopes_behavior property to true.
Creating cluster d4gcluster...done.                                                                                                                                                                        
Created [https://container.googleapis.com/v1/projects/website-192121/zones/europe-west1-b/clusters/d4gcluster].
kubeconfig entry generated for d4gcluster.
NAME        LOCATION        MASTER_VERSION  MASTER_IP       MACHINE_TYPE   NODE_VERSION  NUM_NODES  STATUS
d4gcluster  europe-west1-b  1.7.11-gke.1    35.195.155.233  n1-standard-1  1.7.11-gke.1  3          RUNNING
```

Check on Google Cloud plateform that your cluster is up (cf. endpoints = IP address, ...)


#### Run your Docker image as an app on the cluster

* Start a single instance of "dataforgood/site" and let the container (Flask app) expose port 5000

```shell
$ kubectl run d4gcluster --image=dataforgood/site:latest  --port=5000
deployment "d4gcluster" created
```

* Check the deployement:

```shell
$ kubectl get deployment
NAME         DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
d4gcluster   1         1         1            0           26s

$ kubectl get pods ##(=group of contaniers)
NAME                          READY     STATUS    RESTARTS   AGE
d4gcluster-3513637093-g7t99   1/1       Running   0          1m

$ kubectl cluster-info ## get infos on IP addresses of your cluster
...
```


#### Allow the cluster to serve the app to the world!

With this command, serve the app on port 80 and let the cluster connect to Flask on port 5000.

```shell
$ kubectl expose deployment d4gcluster --type=LoadBalancer --port 80 --target-port 5000
service "d4gcluster" exposed

$ kubectl get services d4gcluster  # This outputs the load balancer IP
NAME         TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
d4gcluster   LoadBalancer   <some-IP-address>   <pending>     80:<some-port>/TCP   12s
$ kubectl get services d4gcluster  # This outputs the load balancer IP
NAME         TYPE           CLUSTER-IP      EXTERNAL-IP      PORT(S)        AGE
d4gcluster   LoadBalancer   <some-IP-address>   <some-IP-address>   80:<some-port>/TCP   1m
```

At this stage, you can check that the site is available using the provided IP followed by the port :80

[http://<some-IP-address>:80](http://<some-IP-address>:80)

To scale the webapp, nothing simpler:

```shell
$ kubectl scale deployment d4gcluster --replicas=4
```

and check your new cluster with:

```shell
$ kubectl get deployment
$ kubectl get pods # several pods should be running
```


### Manage the Domain Name of the webapp

* [https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/](https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/)
* [https://cloud.google.com/dns/quickstart](https://cloud.google.com/dns/quickstart)

#### Configuring your Domain Names with Static IP Addresses

This [tutorial](https://cloud.google.com/kubernetes-engine/docs/tutorials/configuring-domain-name-static-ip ) demonstrates how to expose your web application to the internet on a static external IP address and configure DNS records of your domain name to point to your application. This tutorial assumes you own a registered domain name (such as `dataforgood.no` or `d4g.no`). You can register a domain name through Google Domains or another domain registrar of your choice if you do not have one.


Check what works:

```shell
$ host www.d4g.no
www.d4g.no has address 35.195.143.116

$ host dataforgood.no
dataforgood.no has address 216.239.32.21
dataforgood.no has address 216.239.38.21
...
```
 



### Deploy a new version of your webapp

```shell
$ docker build -t dataforgood/site:latest .
$ docker run -d -p 5000:5000 dataforgood/site  ## optional, but always good to test your new site locally
$ docker push dataforgood/site
$ kubectl get pods
NAME                         READY     STATUS    RESTARTS   AGE
d4gcluster-169632988-ftc1b   1/1       Running   0          11m
$ kubectl delete pod d4gcluster-169632988-ftc1b      ### kubectl delete pod {POD_NAME}
pod "d4gcluster-169632988-ftc1b" deleted
```

This uses Kubernetes to delete your cluster which gets automatically rebuilt.

This triggers a fresh pull and rebuild your site with the latest image available on the cloud registry.

