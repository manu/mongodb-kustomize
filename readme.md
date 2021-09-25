# MongoDB Community edition installation using operator

To create a mongodb cluster clone this repository and follow the below steps

## Create namepace
Modify the namespace.yaml file with your namepace name and run the below command
```
kubectl apply -f namespace.yaml
```
This will create a namespace 'dev' if the file is not modified

## Add the custom resource defenitions of the operator
Add the MonogDB Community Operator CRD to the namesapce 'dev'
```
kustomize build mongo-operator/crd/ | kubectl -n dev apply -f -
```
## Deploy the mongodb operator
Once the CRD is added, the operator needs to be deployed in the namespace 'dev'
```
kustomize build mongo-operator/operator/ | kubectl -n dev apply -f -
```
This will take some time to start the operator pod

## Deploy the mongodb replica set
Add a password to the mongo/.env.secret file
```
cat <<EOF >> mongo/.env.secret
password=yourpasswordstring
EOF
```
Deploy the mongodb replica set using the below command
```
kustomize build mongo/ | kubectl -n dev apply -f -
```
Then wait for some time and see if the pods creation is completed using
```
kubectl get po -n dev
```
