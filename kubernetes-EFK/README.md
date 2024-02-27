# kubernetes-efk
This repo is use in [eazytraining's kubernetes course](https://eazytraining.fr/cours/kubernetes-les-bases-pour-devops) to deploy EFK (ElasticSearch - Fluentd - kibana) stack for logging porpuse.
This code is based on the great [digital ocean tutorial](https://www.digitalocean.com/community/tutorials/how-to-set-up-an-elasticsearch-fluentd-and-kibana-efk-logging-stack-on-kubernetes). So you can read it also for some usefull information.
Here we will only give you the command you need to run to setup the stack.
## Namespace

    kubectl create -f kube-logging.yaml

## ElasticSearch

    kubectl create -f elasticsearch_svc.yaml
    kubectl create -f elasticsearch_statefulset.yaml
    
### Verify elasticsearch

    kubectl rollout status sts/es-cluster --namespace=kube-logging
    kubectl port-forward $(kubectl get pod -l app=elasticsearch -o jsonpath="{.items[0].metadata.labels.statefulset\.kubernetes\.io\/pod-name}"  -n kube-logging) 9200:9200 --namespace=kube-logging --address 0.0.0.0
  
Open the browser and verify that the service is available (http://***node-ip***:9200)

## Kibana

    kubectl create -f kibana.yaml
    
### Verify Kibana

    kubectl rollout status deployment/kibana --namespace=kube-logging
    kubectl port-forward $(kubectl get po --namespace=kube-logging -l app=kibana -o=name) 5601:5601 --namespace=kube-logging --address 0.0.0.0
    
Open the browser and verify that the service is available (http://***node-ip***:5601)

## Fluentd

    kubectl create -f fluentd.yaml
    kubectl get ds --namespace=kube-logging
  
Finally, you can go again to kibana (http://***node-ip***:5601)  and put the index ***logstash-**** and that is all

Currently, this code works only on **minikube** due to dynamic provisionning issue
