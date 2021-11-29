# Start minikube on an isolated bare metal or vm
```
sudo apt install conntrack
minikube start --vm-driver=none
```

# Enable nginx ingress
```
minikube addons enable ingress
```

# Add the Nexus repository and install with the sample values.yaml file
```
helm repo add sonatype https://sonatype.github.io/helm3-charts/
helm install nexus-rm sonatype/nexus-repository-manager -f values.yaml 
```

# Verify nodeport service url on minikube
```
minikube service --url nexus-rm-nexus-repository-manager
```

# Get pod name and get nexus credentials 
```
kubectl get po --output=jsonpath={.items..metadata.name}
eg.
kubectl exec nexus-rm-nexus-repository-manager-65f84644d4-v6p2b -- cat /nexus-data/admin.password
```

# Verify ingress was created
Note that hostname was set to `artifact-store.local` for this demo. Ensure to add a local dns entry for this on your machine that points to the minikube IP.
```
kubectl get ingress
```

# Access nexus from browser
```
http://artifact-store.local
```

# Changes made to default values.yaml file
```
nexus.docker.enabled = true
ingress.enabled = true
ingress.hostRepo = artifact-store.local
```

# Additional information
Nexus Repository Manager Helm Chart: https://artifacthub.io/packages/helm/sonatype/nexus-repository-manager
Nexus Repository Manager Documentation: https://help.sonatype.com/repomanager3