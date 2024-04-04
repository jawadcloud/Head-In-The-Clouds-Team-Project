chmod 400 "ce-tp-key-pair.pem"
➜  Week-11-Final-Project ssh -i "ce-tp-key-pair.pem" ubuntu@ec2-35-177-112-78.eu-west-2.compute.amazonaws.com
The authenticity of host 'ec2-35-177-112-78.eu-west-2.compute.amazonaws.com (35.177.112.78)' can't be established.
ED25519 key fingerprint is SHA256:72XX7BqsAjY0unDCPzakYixtQBCKL0G810HQSkDdPqs.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'ec2-35-177-112-78.eu-west-2.compute.amazonaws.com' (ED25519) to the list of known hosts.
Welcome to Ubuntu 22.04.4 LTS (GNU/Linux 6.5.0-1014-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

  System information as of Wed Apr  3 23:00:53 UTC 2024

  System load:  0.0               Processes:                103
  Usage of /:   48.2% of 7.57GB   Users logged in:          0
  Memory usage: 48%               IPv4 address for docker0: 172.17.0.1
  Swap usage:   0%                IPv4 address for ens5:    10.0.1.33

Expanded Security Maintenance for Applications is not enabled.

43 updates can be applied immediately.
28 of these updates are standard security updates.
To see these additional updates run: apt list --upgradable

Enable ESM Apps to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status



The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

ubuntu@ip-10-0-1-33:~$ sudo cat /var/lib/jenkins/secrets/initialAdminPassword

8b0a2725a9aa4da8ba2c29f912893025
ubuntu@ip-10-0-1-33:~$ aws eks --region eu-west-2 update-kubeconfig --name ce-tp-eks-cluster-test
Added new context arn:aws:eks:eu-west-2:654654377543:cluster/ce-tp-eks-cluster-test to /home/ubuntu/.kube/config
ubuntu@ip-10-0-1-33:~$ kubectl get pods
No resources found in default namespace.
ubuntu@ip-10-0-1-33:~$ kubectl get pods -n ce-tp
NAME                                   READY   STATUS    RESTARTS   AGE
backend-deployment-7fbff98d9b-jf994    1/1     Running   0          47s
frontend-deployment-5cb44f9f69-54s6b   1/1     Running   0          47s
ubuntu@ip-10-0-1-33:~$ kubectl get services -n ce-tp
NAME               TYPE           CLUSTER-IP       EXTERNAL-IP                                                              PORT(S)          AGE
backend-service    LoadBalancer   172.20.99.144    ab7f881b45f6a424397b3d630ae2b35a-527868943.eu-west-2.elb.amazonaws.com   8080:31778/TCP   54s
frontend-service   LoadBalancer   172.20.241.232   ad68e6ac527e64f7f9eff6fc8dede366-3615590.eu-west-2.elb.amazonaws.com     80:31663/TCP     54s
ubuntu@ip-10-0-1-33:~$ kubectl get nodes -n ce-tp
NAME                                      STATUS   ROLES    AGE   VERSION
ip-10-0-4-83.eu-west-2.compute.internal   Ready    <none>   55m   v1.29.0-eks-5e0fdde
ip-10-0-5-50.eu-west-2.compute.internal   Ready    <none>   55m   v1.29.0-eks-5e0fdde
ip-10-0-6-62.eu-west-2.compute.internal   Ready    <none>   55m   v1.29.0-eks-5e0fdde
ubuntu@ip-10-0-1-33:~$ kubectl get deployments -n ce-tp
NAME                  READY   UP-TO-DATE   AVAILABLE   AGE
backend-deployment    1/1     1            1           118s
frontend-deployment   1/1     1            1           118s
ubuntu@ip-10-0-1-33:~$ helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
"prometheus-community" has been added to your repositories
ubuntu@ip-10-0-1-33:~$ helm repo update
Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "prometheus-community" chart repository
Update Complete. ⎈Happy Helming!⎈
ubuntu@ip-10-0-1-33:~$ pw
dpw: command not found
ubuntu@ip-10-0-1-33:~$ pwd
/home/ubuntu
ubuntu@ip-10-0-1-33:~$ vi values.yaml
ubuntu@ip-10-0-1-33:~$ helm install kube-prometheus-stack prometheus-community/kube-prometheus-stack -f values.yaml --namespace monitoring --create-namespace
W0403 23:36:10.045742   12313 warnings.go:70] annotation "kubernetes.io/ingress.class" is deprecated, please use 'spec.ingressClassName' instead
W0403 23:36:10.053730   12313 warnings.go:70] annotation "kubernetes.io/ingress.class" is deprecated, please use 'spec.ingressClassName' instead
NAME: kube-prometheus-stack
LAST DEPLOYED: Wed Apr  3 23:35:59 2024
NAMESPACE: monitoring
STATUS: deployed
REVISION: 1
NOTES:
kube-prometheus-stack has been installed. Check its status by running:
  kubectl --namespace monitoring get pods -l "release=kube-prometheus-stack"

Visit https://github.com/prometheus-operator/kube-prometheus for instructions on how to create & configure Alertmanager and Prometheus instances using the Operator.
ubuntu@ip-10-0-1-33:~$ kubectl get ingress -n monitoring
NAME                               CLASS    HOSTS   ADDRESS   PORTS   AGE
kube-prometheus-stack-grafana      <none>   *                 80      25s
kube-prometheus-stack-prometheus   <none>   *                 80      25s
ubuntu@ip-10-0-1-33:~$ kubectl get service -n monitoring
NAME                                             TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
alertmanager-operated                            ClusterIP   None             <none>        9093/TCP,9094/TCP,9094/UDP   55s
kube-prometheus-stack-alertmanager               ClusterIP   172.20.166.0     <none>        9093/TCP,8080/TCP            62s
kube-prometheus-stack-grafana                    ClusterIP   172.20.137.79    <none>        80/TCP                       62s
kube-prometheus-stack-kube-state-metrics         ClusterIP   172.20.203.207   <none>        8080/TCP                     62s
kube-prometheus-stack-operator                   ClusterIP   172.20.156.31    <none>        443/TCP                      62s
kube-prometheus-stack-prometheus                 ClusterIP   172.20.232.156   <none>        9090/TCP,8080/TCP            62s
kube-prometheus-stack-prometheus-node-exporter   ClusterIP   172.20.190.213   <none>        9100/TCP                     62s
prometheus-operated                              ClusterIP   None             <none>        9090/TCP                     55s
ubuntu@ip-10-0-1-33:~$ cat values.yaml 
grafana:
  ingress:
    enabled: true
    annotations:
      kubernetes.io/ingress.class: alb
      alb.ingress.kubernetes.io/scheme: internet-facing
      alb.ingress.kubernetes.io/target-type: ip
    paths:
      - /*
    pathType: ImplementationSpecific

prometheus:
  ingress:
    enabled: true
    annotations:
      kubernetes.io/ingress.class: alb
      alb.ingress.kubernetes.io/scheme: internet-facing
      alb.ingress.kubernetes.io/target-type: ip
    paths:
      - /*
    pathType: ImplementationSpecific

ubuntu@ip-10-0-1-33:~$ kubectl get ingress -n monitoring
NAME                               CLASS    HOSTS   ADDRESS   PORTS   AGE
kube-prometheus-stack-grafana      <none>   *                 80      4m9s
kube-prometheus-stack-prometheus   <none>   *                 80      4m9s
ubuntu@ip-10-0-1-33:~$ cat values.yaml 
grafana:
  ingress:
    enabled: true
    annotations:
      kubernetes.io/ingress.class: alb
      alb.ingress.kubernetes.io/scheme: internet-facing
      alb.ingress.kubernetes.io/target-type: ip
    paths:
      - /*
    pathType: ImplementationSpecific

prometheus:
  ingress:
    enabled: true
    annotations:
      kubernetes.io/ingress.class: alb
      alb.ingress.kubernetes.io/scheme: internet-facing
      alb.ingress.kubernetes.io/target-type: ip
    paths:
      - /*
    pathType: ImplementationSpecific

ubuntu@ip-10-0-1-33:~$ vi values.yaml 
ubuntu@ip-10-0-1-33:~$ helm upgrade --install kube-prometheus-stack prometheus-community/kube-prometheus-stack -f custom-values.yaml --namespace monitoring --create-namespace
Error: open custom-values.yaml: no such file or directory
ubuntu@ip-10-0-1-33:~$ helm upgrade --install kube-prometheus-stack prometheus-community/kube-prometheus-stack -f values.yaml --
namespace monitoring --create-namespace
Release "kube-prometheus-stack" has been upgraded. Happy Helming!
NAME: kube-prometheus-stack
LAST DEPLOYED: Wed Apr  3 23:47:36 2024
NAMESPACE: monitoring
STATUS: deployed
REVISION: 2
NOTES:
kube-prometheus-stack has been installed. Check its status by running:
  kubectl --namespace monitoring get pods -l "release=kube-prometheus-stack"

Visit https://github.com/prometheus-operator/kube-prometheus for instructions on how to create & configure Alertmanager and Prometheus instances using the Operator.
ubuntu@ip-10-0-1-33:~$ kubectl get ingress -n monitoring
No resources found in monitoring namespace.
ubuntu@ip-10-0-1-33:~$ kubectl get service -n monitoring
NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP                                                              PORT(S)                         AGE
alertmanager-operated                            ClusterIP      None             <none>                                                                   9093/TCP,9094/TCP,9094/UDP      12m
kube-prometheus-stack-alertmanager               ClusterIP      172.20.166.0     <none>                                                                   9093/TCP,8080/TCP               12m
kube-prometheus-stack-grafana                    LoadBalancer   172.20.137.79    ab9fcaf3e0b8b4cfcbee0c29919a096d-277749774.eu-west-2.elb.amazonaws.com   8082:32166/TCP                  12m
kube-prometheus-stack-kube-state-metrics         ClusterIP      172.20.203.207   <none>                                                                   8080/TCP                        12m
kube-prometheus-stack-operator                   ClusterIP      172.20.156.31    <none>                                                                   443/TCP                         12m
kube-prometheus-stack-prometheus                 LoadBalancer   172.20.232.156   af156e1dc75954c43a69bc0b397ffe1f-123917449.eu-west-2.elb.amazonaws.com   8081:31479/TCP,8080:30594/TCP   12m
kube-prometheus-stack-prometheus-node-exporter   ClusterIP      172.20.190.213   <none>                                                                   9100/TCP                        12m
prometheus-operated                              ClusterIP      None             <none>                                                                   9090/TCP                        12m
ubuntu@ip-10-0-1-33:~$ kubectl get pods -n monitoring
NAME                                                        READY   STATUS    RESTARTS   AGE
alertmanager-kube-prometheus-stack-alertmanager-0           2/2     Running   0          14m
kube-prometheus-stack-grafana-7664d8545c-8pzzs              3/3     Running   0          14m
kube-prometheus-stack-kube-state-metrics-5c6549bfd5-m7mvh   1/1     Running   0          14m
kube-prometheus-stack-operator-76bf64f57d-m67rp             1/1     Running   0          14m
kube-prometheus-stack-prometheus-node-exporter-765gs        1/1     Running   0          14m
kube-prometheus-stack-prometheus-node-exporter-7675h        1/1     Running   0          14m
kube-prometheus-stack-prometheus-node-exporter-mgg5z        1/1     Running   0          14m
prometheus-kube-prometheus-stack-prometheus-0               2/2     Running   0          2m33s
ubuntu@ip-10-0-1-33:~$ vi values.yaml 
ubuntu@ip-10-0-1-33:~$ helm upgrade --install kube-prometheus-stack prometheus-community/kube-prometheus-stack -f values.yaml --namespace monitoring --create-namespace
Release "kube-prometheus-stack" has been upgraded. Happy Helming!
NAME: kube-prometheus-stack
LAST DEPLOYED: Wed Apr  3 23:53:03 2024
NAMESPACE: monitoring
STATUS: deployed
REVISION: 3
NOTES:
kube-prometheus-stack has been installed. Check its status by running:
  kubectl --namespace monitoring get pods -l "release=kube-prometheus-stack"

Visit https://github.com/prometheus-operator/kube-prometheus for instructions on how to create & configure Alertmanager and Prometheus instances using the Operator.
ubuntu@ip-10-0-1-33:~$ kubectl get service -n monitoring
NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP                                                              PORT(S)                         AGE
alertmanager-operated                            ClusterIP      None             <none>                                                                   9093/TCP,9094/TCP,9094/UDP      17m
kube-prometheus-stack-alertmanager               ClusterIP      172.20.166.0     <none>                                                                   9093/TCP,8080/TCP               17m
kube-prometheus-stack-grafana                    LoadBalancer   172.20.137.79    ab9fcaf3e0b8b4cfcbee0c29919a096d-277749774.eu-west-2.elb.amazonaws.com   8082:32166/TCP                  17m
kube-prometheus-stack-kube-state-metrics         ClusterIP      172.20.203.207   <none>                                                                   8080/TCP                        17m
kube-prometheus-stack-operator                   ClusterIP      172.20.156.31    <none>                                                                   443/TCP                         17m
kube-prometheus-stack-prometheus                 LoadBalancer   172.20.232.156   af156e1dc75954c43a69bc0b397ffe1f-123917449.eu-west-2.elb.amazonaws.com   8081:31479/TCP,8080:30594/TCP   17m
kube-prometheus-stack-prometheus-node-exporter   ClusterIP      172.20.190.213   <none>                                                                   9100/TCP                        17m
prometheus-operated                              ClusterIP      None             <none>                                                                   9090/TCP                        17m
ubuntu@ip-10-0-1-33:~$ kubectl get pods
No resources found in default namespace.
ubuntu@ip-10-0-1-33:~$ kubectl get pods -n ce-tp
NAME                                  READY   STATUS    RESTARTS   AGE
backend-deployment-7fbff98d9b-jf994   1/1     Running   0          28m
frontend-deployment-697f79f9f-xd9rt   1/1     Running   0          24m
ubuntu@ip-10-0-1-33:~$ sudo nano servicemonitor.yaml
ubuntu@ip-10-0-1-33:~$ kubectl apply -f ce-tp-servicemonitor.yaml --namespace monitoring
error: the path "ce-tp-servicemonitor.yaml" does not exist
ubuntu@ip-10-0-1-33:~$ kubectl apply -f servicemonitor.yaml --namespace monitoring
servicemonitor.monitoring.coreos.com/ce-tp-services created
ubuntu@ip-10-0-1-33:~$ helm upgrade --install kube-prometheus-stack prometheus-community/kube-prometheus-stack -f values.yaml --namespace monitoring
Release "kube-prometheus-stack" has been upgraded. Happy Helming!
NAME: kube-prometheus-stack
LAST DEPLOYED: Thu Apr  4 00:06:53 2024
NAMESPACE: monitoring
STATUS: deployed
REVISION: 4
NOTES:
kube-prometheus-stack has been installed. Check its status by running:
  kubectl --namespace monitoring get pods -l "release=kube-prometheus-stack"

Visit https://github.com/prometheus-operator/kube-prometheus for instructions on how to create & configure Alertmanager and Prometheus instances using the Operator.
ubuntu@ip-10-0-1-33:~$ sudo cat values.yaml 
prometheus:
  prometheusSpec:
    serviceMonitorSelectorNilUsesHelmValues: false
    podMonitorSelectorNilUsesHelmValues: false
    ruleSelectorNilUsesHelmValues: false
  service:
    type: LoadBalancer
    port: 8081
    targetPort: 9090

grafana:
  adminPassword: "password"
  service:
    type: LoadBalancer
    port: 8082  # Changed to 8082 to avoid conflict
    targetPort: 3000

ubuntu@ip-10-0-1-33:~$ sudo cat servicemonitor.yaml 
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: ce-tp-services
  namespace: monitoring # This should match the namespace of your Prometheus
  labels:
    release: kube-prometheus-stack # This label should match your Helm release
spec:
  selector:
    matchLabels:
      app: frontend-service # Change this to match the label of your services
  namespaceSelector:
    matchNames:
      - ce-tp # The namespace you want to monitor
  endpoints:
  - port: metrics
    interval: 30s
ubuntu@ip-10-0-1-33:~$ client_loop: send disconnect: Broken pipe
➜  Week-11-Final-Project 