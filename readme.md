

# Add helm repositories for Prometheus

`kubectl create ns prometheus`

```
helm repo add kube-state-metrics https://kubernetes.github.io/kube-state-metrics
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```
```
helm install prometheus prometheus-community/prometheus \
  --namespace prometheus \
  --set alertmanager.persistentVolume.storageClass="gp2",
  --server.persistentVolume.storageClass="gp2"
```

`kubectl create ns grafana`

```
helm install grafana grafana/grafana \
  --namespace grafana \
  --set persistence.size="40Gi" \
  --set persistence.storageClassName="gp2" \
  --set persistence.enabled=true \
  --set adminPassword='Stro456@Password' \
  --values grapana.yaml \
  --set service.type=NodePort
```


```
helm install aws-load-balancer-controller eks/aws-load-balancer-controller \
--set clusterName=buddy-cluster \
--set vpcId=vpc-0f590c09823b080ac \
--set serviceAccount.create=true \
--set serviceAccount.name=alb-ingress-controller \
--set serviceAccount.annotations."eks\.amazonaws\.com/role-arn"="arn:aws:iam::6994745925445713:role/alb_controller_role" \
--namespace kube-system
```

```
helm install grafana grafana/grafana \
    --namespace grafana \
    --set persistence.storageClassName="gp2" \
    --set persistence.enabled=true
    --set adminPassword='Strong4@Password' \
    --values grapana.yaml \
    --set service.type=NodePort
```
    
```    
helm upgrade -i grafana grafana/grafana \
  --namespace grafana \
  --set persistence.storageClassName="gp2" \
  --set persistence.enabled=true
```

 [References](https://medium.com/@jayvardhanchandel/monitoring-eks-cluster-with-prometheus-and-grafana-a5576301e71b)


Alert

 ```
100 * (1 - (node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes))


100 * (1 - (node_memory_MemAvailable_bytes{instance="sp-app"} / node_memory_MemTotal_bytes{instance="sp-app"}))
100 * (1 - (node_memory_MemAvailable_bytes{instance="sp-database"} / node_memory_MemTotal_bytes{instance="sp-database"}))


100 * (1 - avg(rate(node_cpu_seconds_total{mode="idle", instance="sp-app"}[5m])))
100 * (1 - avg(rate(node_cpu_seconds_total{mode="idle", instance="sp-database"}[5m])))

 ```