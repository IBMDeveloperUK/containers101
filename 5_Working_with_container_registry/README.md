# Working with IBM Cloud Container Registy

So we have created our amazing database, we now want to push it to our private registry. In real agile development, a push to a private registry would trigger a build to run a test suite in a CI/CD pipeline. For this we will be using IBM Container Registry. If you have not created an IBM Cloud account or have not installed the IBM Cloud CLI, [click here](../../../) to follow the steps do so.

First we will need to create a unique namespace for the registry:

`ibmcloud cr namespace-add <my_namespace>`

It will be a good idea to use the same `username` you used when created our custom MySQL image. If this username is taken, just use a value that is unique.

Next, we need to log the local Docker daemon into the IBM Cloud Container Registry:

`ibmcloud cr login`

Take note of the registry that you log in to. It should be in the format: `<region>.icr.io` e.g. `uk.icr.net`

Now we are ready to push our image from our local machine up to the IBM Cloud Container Registry. Before we do so, we will need to tag the image appropriately:

`docker tag <username>/mysql-server <registry>/<my_namespace>/mysql-server`

* The `<username>` field is the same one we used when creating our custom MySQL image.
* The `<registry>` field corresponds to the location returned after running `ibmcloud cr login`.
* The `<my_namespace>` field is the unique identifier we created in the `ibmcloud cr namespace-add` command.
* As we have not specified a tag for the image, it will tag as `latest` by default.

Now we can push our newly tagged custom MySQL image to Cloud Registry:

`docker push <registry>/<my_namespace>/mysql-server`

If you can see the layers of the image being pushed up then you are successfully pushing to IBM Container Registry. Let's go see what you just deployed:

1. Login to [IBM Cloud](https://cloud.ibm.com)
2. Click on the Catalog button on the menu
3. Search for: `container reg` and click on the `Container Registry` service
4. Ensure that you select the correct region e.g. if eu-gb, then make sure the region is London
5. Select `images` and locate the MySQL image we just pushed in the main screen

One of the nice things about Container Registry is that it will scan the image for vulnerabilites everytime an image is pushed to the registry or is updated. Let's click on the link under the `Secuirty Status` column for a summary of the security report. In addition to vulnerability scans, Container Registry shows us configuration issues that we should fix to better secure the container.

Congratulations, you have completed the Containers 101 workshop! You now know how the full Docker ecosystem works from creating a Docker image, creating a container, commiting a new image from a container and both pushing to and pulling from registries. You have also learnt how to work inside containers and control the lifecycle of container.

## What now?

The natural progression from containers is managing a multiple containers in a cluster. This level of thinking adds another level of abstraction on top of Docker with open source solutions such as Kubetnetes. Using your IBM Cloud account, feel free to check out the [IBM Cloud Kubernetes Service](https://cloud.ibm.com/docs/containers/container_index.html#container_index) and the various tutorials to learn how you can start managing containers in a cluster.
