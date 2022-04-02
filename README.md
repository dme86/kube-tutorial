# kube-tutorial

This tutorial requires minikube or K3S up and running.

# Namespaces

A Kubernetes namespace provides the scope for Pods, Services, and Deployments in the cluster. Users interacting with one namespace do not see the content in another namespace.

### Namespaces and Quotas
Create Namespaces:

 - testing
 - development
 - production

with labels 

> customer=foo

> environment={testing,development,production}

    kubectl create -f https://raw.githubusercontent.com/dme86/kube-tutorial/main/namespaces.yml

Apply **quotas** and **limitranges** to **testing** namespace:

    kubectl apply -f https://raw.githubusercontent.com/dme86/kube-tutorial/main/limitrange_quota.yml --namespace=testing

Show **limitrange** from **testing** namespace:

    # only names and creation date
    kubectl get limitrange --namespace=testing
    # full output
    kubectl get limitrange --namespace=testing --output=yaml

Show **resourcequota** *mem-cpu-quota* from **testing** namespace:

    # short output
    kubectl get resourcequota mem-cpu-quota --namespace=testing
    # full yaml output
    kubectl get resourcequota mem-cpu-quota --namespace=testing --output=yaml

# Pods
A pod is a group of one or more containers. A container is an enclosed, self-contained execution process, much like a process in an operating system. Kubernetes uses pods to run your code and images in the cluster.

Kubernetes works with Pods, rather than containers, so that containers in the same pod can be guaranteed to run on the same machine. Containers in the same pod share their networking infrastructure, storage resources, and lifecycle.

### Create a Pod

Create a simple Pod inside **testing**, check it's logs and expose it for access via browser:

##### Create example Pod
    kubectl create -f https://raw.githubusercontent.com/dme86/kube-tutorial/main/pods/nginx-hello.yml --namespace=testing
##### Check it's logs
    kubectl logs nginx-hello --namespace=testing
###### (if using K3S make sure your hostname is defined in /etc/hosts):

    127.0.1.1 babylon.loc babylon
    127.0.0.1 localhost.localdomain localhost
    ::1		  localhost6.localdomain6 localhost6

##### Show Pod-Infos
    kubectl get pods --namespace=testing -o wide
##### Get a Pod description
    kubectl describe po nginx-hello --namespace=testing
##### Expose
    kubectl expose pod nginx-hello --type=NodePort --port=8080 --target-port=80 --namespace=testing
##### Get url and port
    minikube service nginx-hello --url --namespace=testing

###### (or if using K3S):

    kubectl get service -n testing

  

Try to create a second pod from the **pods** folder. The service will expose both pods, see:

    kubectl describe service --namespace=testing

Accessing the pod via browser you'll see the *Server name* - either **nginx-hello** or **nginx-hello-2**. Delete the container wich is displayed on the website and reload: Kubernetes will automatically route the traffic to the other (healthy) container.

##### Delete a Pod

    kubectl delete pod nginx-hello --namespace=testing

# Deployments

A deployment is an object in Kubernetes that lets you manage a set of identical pods.

Without a deployment, you’d need to create, update, and delete a bunch of pods manually.

With a deployment, you declare a single object in a YAML file. This object is responsible for creating the pods, making sure they stay up to date, and ensuring there are enough of them running

You can also easily autoscale your applications using a Kubernetes deployment.

### Create a Deployment

This will create a simple deployment with the same container we used in the Pods section:

    kubectl apply -f https://raw.githubusercontent.com/dme86/kube-tutorial/main/deployments/nginx-deployment.yml --namespace=testing

Check your deployment:

    kubectl get deployments --namespace=testing

