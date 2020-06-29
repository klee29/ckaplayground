### Practice test solution by myself 

1) Practice Test View Certificate Details 
- Identify the certificate file used for the **kube-api server** 
  Check all pods (kubectl get pods --all-namespaces) 
  Check details (kubectl describe pod kube-apiserver-master --namespace=kube-system)

- Identify the Certificate file used to authenticate **kube-apiserver** as a client to **ETCD** server.
  [Check]() 

- Identify the key used to authenticate **kubeapi-server** to the **kubelet** server 
  [Check]()


- Identify the ETCD Server Certificate used to host ETCD server 
  kubectl get pod etcd-master --namespace=kube-system -> [Check]()

- Identify the ETCD Server CA Root Certificate used to serve ETCD Server
  [Check]()

- What is the Common Name (CN) configured on the Kube API Server Certificate? 
  **Not a Issuer**

- What is the name of the CA who issued the Kube API Server Certificate? 
  Issuer 

- Which of the below alternate names is not configured onthe Kube API Server Certificate? 
  Check **X509v3 Subject Alternative Name:** 

- What is the Common Name (CN) congigured on the ETCD Server certificate?
  
- How long, from the issued data, is the Kube-API Server Certificate valid for?

- Kubectl suddenly stops responding to your commands. Check it out! Someone recently modified the **/etc/kubernetes/manifests/etcd.yaml** file 
  
 
