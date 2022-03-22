# kube-tutorial

This tutorial requires minikube up and running

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

