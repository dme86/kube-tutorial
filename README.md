# kube-tutorial

This tutorial requires minikube or K3S up and running

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

### Create a Pod

Create a simple Pod inside **testing**, check it's logs and expose it for access via browser:

##### Create example Pod
    kubectl create -f https://raw.githubusercontent.com/dme86/kube-tutorial/main/pods/nginx-hello.yml --namespace=testing
##### Check it's logs
    kubectl logs nginx-hello --namespace=testing
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
