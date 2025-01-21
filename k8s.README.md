# Helm commands

* Create configuration `helm create <name>`
* Apply initial configuration: `helm install <name> .`
* Apply uptades: `helm upgrade <name> .`

# K8s commands

* Get pods, deployments and services: `kubectl get <pods | deployments | services>`
* Check all pods: `kubectl describe pods`
* Check a pod: `kubectl describe pod <name>`
* Delete pod: `kubectl delete pod <name>`
* Review pods: `kubectl logs <name>`

# Create deployment:
```
kubectl create deployment <name> --image=<registro/url/imagen> --dry-run=client -o yaml > deployment.yml
```

# Create service
```
kubectl create service clusterip <name> --tcp=<8888> --dry-run=client -o yaml > service.yml 
**kubectl create service nodeport <name> --tcp=<3000> --dry-run=client -o yaml > service.yml**
```
* **clusterip**: Only can access into the cluster 
* **nodeport**: Can access outside of the cluster

# Secrets

* Create secrets, Several at a time, one by one.
```
kubectl create secret generic <name> --from-literal=key=value

kubectl create secret generic secret1 --from-literal=key1=value1 --from-literal=key2=value2
```
* Get secrets `kubectl get secrets`
* See the value of a secret `kubectl get secrets <name> -o yaml`

## Edit a secret
The easiest way is deleting it or re-creating it but if it is more than one secret, we do not want to loose the others. Remember that the secrets are in `base64`, if we want to edit them we must do it in `base64`.

1. Edit a secret with `kubectl edit secret <name>` Thiis will invoke the editor.
2. Change the value (You can use the editor [Line to transfor to base64
](https://www.rapidtables.com/web/tools/base64-decode.html))
3. Type **i** to insert ilnes and edit the file
4. Insert the valie to decode in one line
5. Press **esc** and then `:. ! base64 -D` To decode the value
6. Press **i** to insert o edit the value
7. Press **esc** and then `:. ! base64` to decode the value
8. Edit the file again **i** and let the line in its position
9. Press **esc** and then **:wq** to save and exit.


## Set up the google cloud secrets to get the images

1. Create secret:
```
kubectl create secret docker-registry gcr-json-key --docker-server=server-of-GOOGLE-docker.pkg.dev --docker-username=_json_key --docker-password="$(cat 'PATH/of/store Microservices IAM.json')" --docker-email=YOUR_EMAIL@gmail.com
```

2. Path of the secret to use the key:
```
kubectl patch serviceaccounts default -p '{ "imagePullSecrets": [{ "name":"gcr-json-key" }] }'
```


## Export and apply the configuration with files (secrets in this case)
* To export the files of configuration

```
kubectl get secret <name> -o yaml > <name>.yml
```

* Apply the configuration based on the file
```
kubectl create -f <name>.yml
```

