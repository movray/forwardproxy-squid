# forwardproxy-squid

## Install
simply first apply the config map then the deployment
In the deployment file the service Object will also be created


     kubectl apply configMap.yaml
     kubectl apply squid-deployment.yaml



## How it is implmented
- uses the official Image from Ubuntu: ```ubuntu/squid:4.13-21.10_edge```
- creates a pod-port at 3128
- creates a service and binds this pod-port to a clusterIP on port 3128



## Test
For testing purposes bind the service port to localhost

```kubectl port-forward service/squid-service 3128 3128```


## hints/tips
- If you change the configmap, then the pod must be recrated. For that i simply delete the pod, k8s recreates a pod from the replicaset.


## forward proxy redirects
the repo has a default configuration with tree redirects to test with

every redirect block looks like this

    acl redir1 dstdomain .debian.org
    deny_info http://openbsd.org redir1 
    http_reply_access deny redir1 

in this example all request going to debian.org (and/or to subdomains of it) will be forwarded with a 302 code to openbsd.org 
The variable redir1 squid needs this to determine the identifer (acl) for this redirect. This (acl) identifier must be unique for every redirect block.
If you want to add more redirects then extend the config map with adjusted values of this tree lines.
