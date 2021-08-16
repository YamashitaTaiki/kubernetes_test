** 容量は大きめじゃないとminikube起動でエラーになります

・kubectlコマンドダウンロード
curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl"
・実行権限
chmod +x ./kubectl
・pathに移動
sudo mv ./kubectl /usr/local/bin/kubectl
・バージョン確認（kubectlコマンドが使用できること）
kubectl version --client

・dockerインストール
sudo apt-get update &&     sudo apt-get install docker.io -y
・minikubeコマンドダウンロード
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
・実行権限付与
chmod +x minikube
・pathに移動
sudo mv minikube /usr/local/bin/
・バージョン確認（minikubeコマンドが使用できること）
minikube version

・管理者にスイッチ
sudo -i
   
・conntrackをインストール
apt install conntrack


* 以降minikube起動からnginx公開までのコマンド履歴

root@ip-172-31-0-143:~# minikube start --vm-driver=none
* minikube v1.22.0 on Ubuntu 20.04
* Using the none driver based on user configuration
* Starting control plane node minikube in cluster minikube
* Running on localhost (CPUs=2, Memory=3876MB, Disk=7876MB) ...
* OS release is Ubuntu 20.04.2 LTS
* Preparing Kubernetes v1.21.2 on Docker 20.10.7 ...
  - kubelet.resolv-conf=/run/systemd/resolve/resolv.conf
    > kubectl.sha256: 64 B / 64 B [--------------------------] 100.00% ? p/s 0s
    > kubeadm.sha256: 64 B / 64 B [--------------------------] 100.00% ? p/s 0s
    > kubelet.sha256: 64 B / 64 B [--------------------------] 100.00% ? p/s 0s
    > kubectl: 44.27 MiB / 44.27 MiB [------------] 100.00% 87.61 MiB p/s 700ms
    > kubeadm: 42.57 MiB / 42.57 MiB [-------------] 100.00% 44.03 MiB p/s 1.2s
    > kubelet: 112.68 MiB / 112.68 MiB [-----------] 100.00% 82.34 MiB p/s 1.6s
  - Generating certificates and keys ...
  - Booting up control plane ...
  - Configuring RBAC rules ...
* Configuring local host environment ...
*
! The 'none' driver is designed for experts who need to integrate with an existing VM
* Most users should use the newer 'docker' driver instead, which does not require root!
* For more information, see: https://minikube.sigs.k8s.io/docs/reference/drivers/none/
*
! kubectl and minikube configuration will be stored in /root
! To use kubectl or minikube commands as your own user, you may need to relocate them. For example, to overwrite your own settings, run:
*
  - sudo mv /root/.kube /root/.minikube $HOME
  - sudo chown -R $USER $HOME/.kube $HOME/.minikube
*
* This can also be done automatically by setting the env var CHANGE_MINIKUBE_NONE_USER=true
* Verifying Kubernetes components...
  - Using image gcr.io/k8s-minikube/storage-provisioner:v5
* Enabled addons: storage-provisioner, default-storageclass
* Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
root@ip-172-31-0-143:~# history
    1  apt install conntrack
    2  minikube start --vm-driver=none
    3  history
root@ip-172-31-0-143:~# minikube status
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured

root@ip-172-31-0-143:~# kubectl create deployment --image nginx my-nginx
deployment.apps/my-nginx created
root@ip-172-31-0-143:~# kubectl get pods
NAME                        READY   STATUS              RESTARTS   AGE
my-nginx-6b74b79f57-88nbw   0/1     ContainerCreating   0          8s
root@ip-172-31-0-143:~# kubectl get deployment
NAME       READY   UP-TO-DATE   AVAILABLE   AGE
my-nginx   1/1     1            1           14s
root@ip-172-31-0-143:~# kubectl scale deployment --replicas 2 my-nginx
deployment.apps/my-nginx scaled
root@ip-172-31-0-143:~# kubectl get pods
NAME                        READY   STATUS    RESTARTS   AGE
my-nginx-6b74b79f57-88nbw   1/1     Running   0          30s
my-nginx-6b74b79f57-ggsl2   1/1     Running   0          5s

・サービス設定
kubectl expose deployment my-nginx --port=80 --type=LoadBalancer
・サービス確認
kubectl get services
```
