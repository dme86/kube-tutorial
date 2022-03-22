# kube-tutorial

This tutorial requires minikube up and running

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

