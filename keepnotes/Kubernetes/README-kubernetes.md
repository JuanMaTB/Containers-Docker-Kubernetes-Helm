## Before to start...
I used Google cloud editor and also its internal linux machine to make this deployment by 3 next reasons:
  * Avoid compatibilities issues
  * Latest tools versions (kubectl and Helm)
  * It is real.

# First steps:

In order to deploy this App we need create a GCP Cluster and stablish connection with it.

Something like that must be pasted in your terminal

```gcloud container clusters get-credentials keepontesftw --zone us-west1-b --project vocal-ceiling-XXXXX```

Then in order to make Ingress work properly, we will need use next command:

```kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.3.1/deploy/static/provider/cloud/deploy.yaml```

then we must take note of external IP after use this command

```kubectl get all --namespace=ingress-nginx```
and copy under spec/rules umbrella in our ingress.yaml using this format (this just an example)

spec:/rules:
  - host: ****34-71-19-250.nip.io****
 
# Deploying manifests

Time to use kubectl to use our manifests.yaml

In order to avoid "hardcode" I used base64 encription so be sure to build your users and passwords (and modify secret.yaml if you need), so you may use:
```echo -n 'user or root or pass or whatever you need...' | base64```

### Lets start.
    
    ```kubectl apply -f app-hpa.yaml
    kubectl apply -f configmap-db.yaml
    kubectl create -f db-secrets.yaml
    kubectl create -f configmap-table.yaml
    kubectl create -f app-secrets.yaml
    kubectl create -f svc-db.yaml
    kubectl create -f deploy-app.yaml
    kubectl create -f deploy-db.yaml
    kubectl create -f pvc.yaml
    kubectl create -f ingress.yaml
    kubectl delete svc-app.yaml```

### Checking

To verify, lets do some actions before conect to our app and use it:

In order to see if our Pods raised properly:

    ```kubectl get pods```
    
In order to see if our services has endopoint:

    ```kubectl get svc```
    
Once we have the ID of the POD:

    ```kubectl describe svc <pod-name>```
    
Or check logs with following command

  ```kubectl get pods | grep mysql; kubectl logs -f <pod-name>```
  
        
As Kubernetes, is a good practice check your database.

```kubectl exec --stdin --tty <pod-name> -- /bin/sh```
    
    
### check inside!!


![tabla funciona](https://user-images.githubusercontent.com/107815913/193477714-370fa82a-9cc7-4ada-a26e-e372cd23a2ca.PNG)


Now, its time to wait, even we can see our webapp we cant connect our database. If you see something like ```Mysql cannot access "db" -2``` you need to wait until db will created





