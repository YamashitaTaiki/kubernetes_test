```
$ kubectl cluster-info
Kubernetes master is running at https://127.0.0.1:46253
KubeDNS is running at https://127.0.0.1:46253/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'

//start.spring.io を使用して、アプリケーションを最初から作成
curl https://start.spring.io/starter.tgz -d dependencies=webflux,actuator | tar -xzvf

//OSシステム更新
apt -y update
   
//openjdk-11インストール
apt -y install openjdk-11-jdk
   
//バージョン確認
root@ip-172-31-0-143:~# java -version
openjdk version "11.0.11" 2021-04-20
OpenJDK Runtime Environment (build 11.0.11+9-Ubuntu-0ubuntu2.20.04)
OpenJDK 64-Bit Server VM (build 11.0.11+9-Ubuntu-0ubuntu2.20.04, mixed mode, sharing)
   
//アプリケーションビルド
./mvnw install

//gitインストール
apt -y install git

//ビルドの結果を確認
ls -l target/*.jar

//jar実行
 java -jar target/*.jar
 
//mavenでアプリケーションをコンテナ化する （demo:0.0.1-SNAPSHOTができる）
./mvnw spring-boot:build-image

//springでビルドしたコンテナ起動
docker run -p 8080:8080 demo:0.0.1-SNAPSHOT

//イメージをタグ付け
docker tag demo:0.0.1-SNAPSHOT springguides/demo

//イメージをプッシュ （docker loginが必要）
docker push springguides/demo

//deploymentを作成
kubectl create deployment demo --image=springguides/demo --dry-run=client -o=yaml > deployment.yaml

echo --- >> deployment.yaml
//サービスを作成
kubectl create service clusterip demo --tcp=8080:8080 --dry-run=client -o=yaml >> deployment.yaml

//ポッドを生成する
kubectl apply -f deployment.yaml

//ポッドを生成できたか確認 （deploymentsとservice）
kubectl get all

//ポートフォワードする(podにアクセスできるようにする)
kubectl port-forward svc/demo 8080:8080

//socatが足りなかったのでインストールした
apt-get -y install socat

```
