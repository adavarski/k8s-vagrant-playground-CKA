# Kubernetes CKA Example Playground Environments

This is based on https://levelup.gitconnected.com/kubernetes-cka-example-questions-practical-challenge-86318d85b4d for setting up local cluster using Vagrant + Virtualbox

# Updates: Base repo is updated for below

- Updated for kubernetes 1.19.1
- Added containerd as container runtime
- Updated scripts 

## Challenges:

- Multi Container Issue: https://medium.com/faun/kubernetes-cka-hands-on-challenge-1-multi-container-issue-5a8c007686ed
- Scheduler Playground: https://levelup.gitconnected.com/kubernetes-cka-hands-on-challenge-2-scheduler-playground-f6c0ea7389ca
- Advanced Scheduling: https://codeburst.io/kubernetes-cka-hands-on-challenge-3-advanced-scheduling-3fbeb67f2f2
- Node Management: https://codeburst.io/kubernetes-cka-hands-on-challenge-4-node-management-df7bf48897d3
- Manage Certificates: https://codeburst.io/kubernetes-cka-hands-on-challenge-5-manage-certificates-8d756d842138
- Pod Priority: https://medium.com/faun/kubernetes-cka-hands-on-challenge-6-pod-priority-1fe95f613ac5
- RBAC: https://medium.com/faun/kubernetes-cka-hands-on-challenge-7-rbac-ac1cf1684dd5
- Etc.

## Setup and Run

You will start a two node cluster on your machine, one master and one worker.

For this you need to install VirtualBox and vagrant, then: go to relative cluster i.e. docker based or containerd based and boot up

./up.sh

## Save the state

vagrant suspend

Examples (and checks):

```
$ cd cluster-docker; ./up.sh
$ vagrant status
Current machine states:

cluster1-master1          running (virtualbox)
cluster1-worker1          running (virtualbox)

This environment represents multiple VMs. The VMs are all listed
above with their current state. For more information about a specific
VM, run `vagrant status NAME`.
$ vagrant ssh cluster1-master1
Welcome to Ubuntu 18.04.5 LTS (GNU/Linux 4.15.0-123-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Thu Nov 12 17:20:18 UTC 2020

  System load:  0.27              Users logged in:        0
  Usage of /:   31.5% of 9.63GB   IP address for enp0s3:  10.0.2.15
  Memory usage: 40%               IP address for enp0s8:  192.168.101.101
  Swap usage:   0%                IP address for docker0: 172.17.0.1
  Processes:    140               IP address for weave:   10.32.0.1


3 packages can be updated.
3 updates are security updates.

New release '20.04.1 LTS' available.
Run 'do-release-upgrade' to upgrade to it.


Last login: Thu Nov 12 17:18:47 2020 from 10.0.2.2

vagrant@cluster1-master1:~$ kubectl cluster-info
Kubernetes master is running at https://192.168.101.101:6443
KubeDNS is running at https://192.168.101.101:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
vagrant@cluster1-master1:~$ kubectl get node
NAME               STATUS   ROLES    AGE     VERSION
cluster1-master1   Ready    master   5m50s   v1.19.4
cluster1-worker1   Ready    <none>   3m7s    v1.19.4
vagrant@cluster1-master1:~$ kubectl get ns
NAME              STATUS   AGE
default           Active   5m52s
development       Active   2m5s
kube-node-lease   Active   5m54s
kube-public       Active   5m54s
kube-system       Active   5m54s
management        Active   2m4s
vagrant@cluster1-master1:~$ kubectl get pods -o wide --sort-by="{.spec.nodeName}" --all-namespaces
NAMESPACE     NAME                                       READY   STATUS              RESTARTS   AGE     IP                NODE               NOMINATED NODE   READINESS GATES
kube-system   coredns-f9fd979d6-gtdfp                    1/1     Running             0          5m48s   10.32.0.2         cluster1-master1   <none>           <none>
kube-system   weave-net-2fw6b                            2/2     Running             0          5m48s   192.168.101.101   cluster1-master1   <none>           <none>
kube-system   kube-scheduler-cluster1-master1            1/1     Running             0          5m57s   192.168.101.101   cluster1-master1   <none>           <none>
kube-system   kube-scheduler-amazing-cluster1-master1    1/1     Running             0          5m56s   192.168.101.101   cluster1-master1   <none>           <none>
kube-system   kube-proxy-g9qdt                           1/1     Running             0          5m48s   192.168.101.101   cluster1-master1   <none>           <none>
kube-system   kube-controller-manager-cluster1-master1   1/1     Running             0          5m57s   192.168.101.101   cluster1-master1   <none>           <none>
kube-system   kube-apiserver-cluster1-master1            1/1     Running             0          5m57s   192.168.101.101   cluster1-master1   <none>           <none>
kube-system   etcd-cluster1-master1                      1/1     Running             0          5m57s   192.168.101.101   cluster1-master1   <none>           <none>
kube-system   coredns-f9fd979d6-q86vq                    1/1     Running             0          5m48s   10.32.0.3         cluster1-master1   <none>           <none>
default       web-test-2-594487698d-s8hf5                1/1     Running             0          2m17s   10.44.0.9         cluster1-worker1   <none>           <none>
development   what-a-deployment-f6b9bbf8c-ttl2t          1/1     Running             0          2m17s   10.44.0.8         cluster1-worker1   <none>           <none>
development   what-a-deployment-f6b9bbf8c-2d4nc          1/1     Running             0          2m17s   10.44.0.4         cluster1-worker1   <none>           <none>
development   web-dev-shop-dev2-679468cd95-hxk6c         1/1     Running             0          2m17s   10.44.0.6         cluster1-worker1   <none>           <none>
development   web-dev-shop-dev2-679468cd95-8ppsd         1/1     Running             0          2m17s   10.44.0.10        cluster1-worker1   <none>           <none>
kube-system   kube-proxy-7rrz2                           1/1     Running             0          3m24s   192.168.101.201   cluster1-worker1   <none>           <none>
development   web-dev-shop-5cc79c745c-dmpfs              1/1     Running             0          2m17s   10.44.0.5         cluster1-worker1   <none>           <none>
default       web-test-6c77dcfbc-kcgz2                   1/1     Running             0          2m17s   10.44.0.1         cluster1-worker1   <none>           <none>
default       web-test-6c77dcfbc-hkpn7                   1/1     Running             0          2m17s   10.44.0.2         cluster1-worker1   <none>           <none>
default       web-test-6c77dcfbc-2t2p5                   1/1     Running             0          2m17s   10.44.0.3         cluster1-worker1   <none>           <none>
kube-system   weave-net-z5wnd                            2/2     Running             1          3m24s   192.168.101.201   cluster1-worker1   <none>           <none>
management    important-pod                              1/1     Running             0          2m17s   10.44.0.11        cluster1-worker1   <none>           <none>
management    m-2x3-api-66c6b9c654-6pt7r                 1/1     Running             0          2m17s   10.44.0.18        cluster1-worker1   <none>           <none>
management    m-2x3-api-66c6b9c654-t6v2x                 1/1     Running             0          2m17s   10.44.0.13        cluster1-worker1   <none>           <none>
management    m-2x3-web-6dd67bb7b-5w5qs                  1/1     Running             0          2m17s   10.44.0.14        cluster1-worker1   <none>           <none>
management    m-2x3-web-6dd67bb7b-cczkc                  1/1     Running             0          2m17s   10.44.0.12        cluster1-worker1   <none>           <none>
management    m-2x3-web-6dd67bb7b-p4vvj                  1/1     Running             0          2m17s   10.44.0.7         cluster1-worker1   <none>           <none>
management    m-3cc-runner-5f9d7d78b9-k5wwj              1/1     Running             0          2m16s   10.44.0.15        cluster1-worker1   <none>           <none>
management    m-3cc-runner-5f9d7d78b9-l6d6q              1/1     Running             0          2m17s   10.44.0.20        cluster1-worker1   <none>           <none>
management    m-3cc-runner-heavy-76c7849ff5-f4l2g        1/1     Running             0          2m16s   10.44.0.19        cluster1-worker1   <none>           <none>
management    m-3cc-runner-heavy-76c7849ff5-zvfgb        1/1     Running             0          2m16s   10.44.0.16        cluster1-worker1   <none>           <none>
management    web-server                                 0/2     ContainerCreating   0          2m17s   <none>            cluster1-worker1   <none>           <none>
vagrant@cluster1-master1:~$ exit

$ ./down.sh

$ cd cluster-containerd; ./up.sh

$ vagrant status
Current machine states:

controlplane              running (virtualbox)
worker1                   running (virtualbox)

This environment represents multiple VMs. The VMs are all listed
above with their current state. For more information about a specific
VM, run `vagrant status NAME`.
$ vagrant ssh controlplane
Welcome to Ubuntu 18.04.5 LTS (GNU/Linux 4.15.0-123-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Thu Nov 12 17:34:12 UTC 2020

  System load:  0.54              Users logged in:        0
  Usage of /:   31.5% of 9.63GB   IP address for enp0s3:  10.0.2.15
  Memory usage: 41%               IP address for enp0s8:  192.168.101.104
  Swap usage:   0%                IP address for docker0: 172.17.0.1
  Processes:    139               IP address for weave:   10.32.0.1


3 packages can be updated.
3 updates are security updates.

New release '20.04.1 LTS' available.
Run 'do-release-upgrade' to upgrade to it.


Last login: Thu Nov 12 17:30:56 2020 from 10.0.2.2
vagrant@controlplane:~$ kubectl cluster-info
Kubernetes master is running at https://192.168.101.104:6443
KubeDNS is running at https://192.168.101.104:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
vagrant@controlplane:~$ kubectl get node
NAME           STATUS   ROLES    AGE     VERSION
controlplane   Ready    master   7m37s   v1.19.4
worker1        Ready    <none>   4m50s   v1.19.4
vagrant@controlplane:~$ kubectl get ns
NAME              STATUS   AGE
default           Active   7m56s
development       Active   4m5s
kube-node-lease   Active   7m58s
kube-public       Active   7m58s
kube-system       Active   7m58s
management        Active   4m5s
vagrant@controlplane:~$ kubectl get pods -o wide --sort-by="{.spec.nodeName}" --all-namespaces
NAMESPACE     NAME                                   READY   STATUS    RESTARTS   AGE     IP                NODE           NOMINATED NODE   READINESS GATES
kube-system   coredns-f9fd979d6-2fpx5                1/1     Running   0          7m48s   10.32.0.3         controlplane   <none>           <none>
kube-system   weave-net-wtklp                        2/2     Running   0          7m48s   192.168.101.104   controlplane   <none>           <none>
kube-system   kube-scheduler-controlplane            1/1     Running   0          7m58s   192.168.101.104   controlplane   <none>           <none>
kube-system   kube-scheduler-amazing-controlplane    1/1     Running   0          7m57s   192.168.101.104   controlplane   <none>           <none>
kube-system   kube-proxy-7p892                       1/1     Running   0          7m48s   192.168.101.104   controlplane   <none>           <none>
kube-system   kube-controller-manager-controlplane   1/1     Running   0          7m58s   192.168.101.104   controlplane   <none>           <none>
kube-system   kube-apiserver-controlplane            1/1     Running   0          7m58s   192.168.101.104   controlplane   <none>           <none>
kube-system   etcd-controlplane                      1/1     Running   0          7m58s   192.168.101.104   controlplane   <none>           <none>
kube-system   coredns-f9fd979d6-zpdph                1/1     Running   0          7m48s   10.32.0.2         controlplane   <none>           <none>
default       web-test-2-594487698d-x9nfq            1/1     Running   0          4m15s   10.44.0.3         worker1        <none>           <none>
development   what-a-deployment-f6b9bbf8c-px5gf      1/1     Running   0          4m15s   10.44.0.6         worker1        <none>           <none>
development   what-a-deployment-f6b9bbf8c-bpg8f      1/1     Running   0          4m15s   10.44.0.10        worker1        <none>           <none>
development   web-dev-shop-dev2-679468cd95-qrcbh     1/1     Running   0          4m15s   10.44.0.4         worker1        <none>           <none>
development   web-dev-shop-dev2-679468cd95-jg56z     1/1     Running   0          4m15s   10.44.0.7         worker1        <none>           <none>
development   web-dev-shop-5cc79c745c-jvhnm          1/1     Running   0          4m15s   10.44.0.8         worker1        <none>           <none>
kube-system   kube-proxy-b77df                       1/1     Running   0          5m21s   192.168.101.204   worker1        <none>           <none>
default       web-test-6c77dcfbc-q6dvg               1/1     Running   0          4m15s   10.44.0.5         worker1        <none>           <none>
default       web-test-6c77dcfbc-phxvq               1/1     Running   0          4m15s   10.44.0.2         worker1        <none>           <none>
kube-system   weave-net-v7g8l                        2/2     Running   1          5m21s   192.168.101.204   worker1        <none>           <none>
default       web-test-6c77dcfbc-nsf4z               1/1     Running   0          4m15s   10.44.0.1         worker1        <none>           <none>
management    important-pod                          1/1     Running   0          4m15s   10.44.0.9         worker1        <none>           <none>
management    m-2x3-api-66c6b9c654-jndbq             1/1     Running   0          4m14s   10.44.0.12        worker1        <none>           <none>
management    m-2x3-api-66c6b9c654-m75vm             1/1     Running   0          4m14s   10.44.0.13        worker1        <none>           <none>
management    m-2x3-web-6dd67bb7b-4r9sb              1/1     Running   0          4m14s   10.44.0.15        worker1        <none>           <none>
management    m-2x3-web-6dd67bb7b-9clfv              1/1     Running   0          4m14s   10.44.0.16        worker1        <none>           <none>
management    m-2x3-web-6dd67bb7b-m55d6              1/1     Running   0          4m14s   10.44.0.17        worker1        <none>           <none>
management    m-3cc-runner-5f9d7d78b9-gd8cf          1/1     Running   0          4m14s   10.44.0.14        worker1        <none>           <none>
management    m-3cc-runner-5f9d7d78b9-rgmk9          1/1     Running   0          4m14s   10.44.0.18        worker1        <none>           <none>
management    m-3cc-runner-heavy-76c7849ff5-blq8m    1/1     Running   0          4m14s   10.44.0.20        worker1        <none>           <none>
management    m-3cc-runner-heavy-76c7849ff5-jh7gn    1/1     Running   0          4m14s   10.44.0.19        worker1        <none>           <none>
management    web-server                             1/2     Error     0          4m15s   10.44.0.11        worker1        <none>           <none>


```


