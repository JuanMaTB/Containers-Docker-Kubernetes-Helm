# HELM

Its time to build a nice template from our APP.

Sometimes we need change some variables in our APPs, and this is what Helm is going to do by us.

Remember prepare te field for your ingress in order to reach your appweb by internet.

```kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.3.1/deploy/static/provider/cloud/deploy.yaml```

and verify external IP in order to modify ingress in values.yaml

```kubectl get svc --namespace=ingress-nginx```

In this moments, before to deploy, we must set our own values in values.yaml

Once we got it just set in a directory above keepnotes dir and use next command

```helm install nicename keepnotes```

You will see how our app its deploying. 


![helmcreado](https://user-images.githubusercontent.com/107815913/193477855-c2c60fd5-fc61-433a-ae6d-7e821ffdfb8b.PNG)



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


Now, its time to wait, even we can see our webapp we cant connect our database. If you see something like ```Mysql cannot access "db" -2``` you need to wait until db will be created

Enjoi
