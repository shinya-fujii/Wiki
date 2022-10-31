# Kubernetes

# 目次

# 参考

[コンセプト](https://kubernetes.io/ja/docs/concepts/)

[僕らは何故Kubernetesを使うのか](https://zenn.dev/esaka/articles/2d117655af1f03cf2444)

[Kubernetesとは何かを図でわかりやすく解説！Pod、Na...｜Udemy メディア](https://udemy.benesse.co.jp/development/system/kubernetes.html/amp)

# Kuberbetes(クーベネティス)とは

---

`Kubernetes`とは宣言的にインフラの構成管理・自動化を促進するコンテナオーケストレーションツールの１つ。

### 概要

- 公式からの引用
    
    > Kubernetesを機能させるには、*Kubernetes API オブジェクト*を使用して、実行したいアプリケーションやその他のワークロード、使用するコンテナイメージ、レプリカ(複製)の数、どんなネットワークやディスクリソースを利用可能にするかなど、クラスターの *desired state*(望ましい状態)を記述します。desired state (望ましい状態)をセットするには、Kubernetes APIを使用してオブジェクトを作成します。通常はコマンドラインインターフェイス `kubectl`を用いてKubernetes APIを操作しますが、Kubernetes APIを直接使用してクラスターと対話し、desired state (望ましい状態)を設定、または変更することもできます。
    > 

コンテナ環境の理想状態のconfiglationを受け取るとそれを実現してくれる。

理想状態はyamlで渡す。ここにはpodの設定が記述されている。詳しくはこれから説明。

podに障害があり、理想状態と現実状態に乖離が発生すると自動的に理想状態になるようpodを起動する

### コンテナオーケストレーションとは何か？

宣言的に設定することでこういった悩みを解決する。

このコンテナオーケストレーションツールができる前までは、それぞれ以下の機能を担うツールが別々で利用されていた。

- 主な機能
    1. `デプロイメント`
        
        どのようにコンテナをデプロイするのか？を定義することでデプロイ自動的に行ってくれる。
        
    2. `スケジューリング`
        
        どのマシン上で動かすのか？地理的に分散・マシンのスペックなどの用件
        
    3. `オートスケーリング`
        
        コンテナの数の管理。負荷が増えれは増やして分散
        
    4. `ネットワーク`
        
        複数のコンテナ間のロードバランスやコンテナ間の通信を行う。
        
    5. `リソースマネジメント`
        
        使用可能なリソースを環境ごとに定義・デフォルト値を設定したりできる
        
    6. `セキュリティ`
        
        どこからどこへのアクセスを許すかなどのネットワークポリシーの設定。
        
        各リソースの権限の定義を行う。
        

### Dockerとの違い

`**Docker**`ではシングルノードを活用し、

`**Kubernetes**`ではクラスター間での実行がメインの違いになると考えて良い。

### kubernetesの利用方法

Kubernetesを機能させるには`**Kubernetes API オブジェクト`** を利用する。

基本的には`kubectl`を用いてKubernetes APIを操作する。

# Kubenetesの構造

---

## Kubernetes Cluster

clusterと呼ばれるのが最上位にある階層。複数のコンテナを管理するpotを制御する。

主な2つのコンポーネントで構成されている。

- **Control Plain(コントロールプレイン)**
- **Worker Node(ワーカーノード)**

### Worker **Node (ノード)**

コンテナが実行されるマシンが必要になるが、それはコンテナランタイムがインストールされている必要がある。

Kubeではそれが`**WorkerNode`** と呼ばれ、こちらのリソースを使用しコンテナが動作する各マシンを管理している。

ワーカーノードはクラスタ内に複数あるイメージ

### Controll Plain(コントロールプレイン)

運用者がやっていたことをシステムが肩代わりしてくれるが、そのシステムを動かすためのマシンを**`Controll Plain`**または**`master`**と呼ばれる。

**ワーカーノード上のコンテナの状態を保持し、管理する役割を持っている**

デプロイメント・スケジューリングなど

# オーケストレーション機能を支える部分

---

## 1. Node上に存在する2つのコンポーネント

### `kubelet`

NodeからControll Plainへ疎通するためのもの。

自分のコンテナを起動・停止したり、コンテナの情報を伝えたりする。

### `kube-proxcy`

Nodeのネットワークのルールを管理している。

kubernetesの設定から実際のルーティングに置き換える働きをしている。

## 2. Controll Plain上に存在する5つのコンポーネント

### `Kube-API-Server(kpi-server)`

Nodeの`kubelet` と通信を行うAPIサーバー

### `etcd`

kubernetes内の状態を保存しておくためのkey:valueで保持するデータベース。

以下の状態を管理する。

kubernetes内にいくつのNodeが管理されているのか？

各Nodeにどのアプリケーションが動いているのか？

リソースがどのくらい利用されているのか？

### `Kube Scheduler`

名前の通りアプリケーションをどのWorkerNode上で実行するのかスケジューリングする。

### `kube controller manager`

KubernetesのControllerは理想状態と現状を比較し、違いがあれば理想状態に近づけようとするもの。

例えば、nginxの理想状態がレプリカ数を３とセットしていれば、2つしか動いていない場合に自動的に1つ追加して3つに近づけようとする。

### `cloud controll manager`

cloud provider(GCPなど？)のAPIと連携して、kubernetesクラスターがcloud providerのリソースを管理できるもの。

ローカルで使う場合には不要。

## 3. これらのコンポーを操作するkubectl

`kubectl` は指定したkubernetesクラスターの`kube-api-server`のクライアントとして動作するコマンドラインツール。

`kubectl get po` などのコマンドを叩くと`kube-api-server` へリクエストし結果を返してくれる。

# kubernetesの主なリソース

---

## NameSpace

## **Pod**

- 公式
    
    [Podの概観](https://kubernetes.io/ja/docs/concepts/workloads/pods/pod-overview/)
    

<aside>
⚠️ 単一のPod内のコンテナ群は、クラスター内において同一の物理マシンもしくは仮想マシン上において自動で同じ環境に配備され、スケジュールされます。

</aside>

Podとは、kubernetes上にデプロイ可能な最小単位のオブジェクト。

**複数のコンテナを含むことができる**。

### ストレージ

- Podは共有されたストレージボリュームのセットを指定できる。
- Pod内の全てのコンテナは、その共有されたボリュームにアクセスできコンテナ間でデータを共有することができる
- ボリュームもまた、もしPod内のコンテナの1つが再起動が必要になった場合に備えて、データを永続化できる

### ネットワーク

- 各Podは固有のIPアドレスを割り当てられる。
- 単一のPod内の各コンテナは、IPアドレスやポート、そのネットワークの名前空間を共有する。
- Pod内のコンテナは`localhost`を使用してお互いに疎通できる
- Pod外のエンティティと疎通する場合、共有されたネットワークリソース(ポートなど）をどのように使うかに関して調整しなければならない

### Podのテンプレート

```
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
spec:
  containers:
  - name: myapp-container
    image: busybox
    command: ['sh', '-c', 'echo Hello Kubernetes! && sleep 3600']
```

## **ReplicaSet**

日本語では「レプリカセット」とも呼ばれており、Podのレプリカの作成やPodの起動数の管理リソース機能です。

## **Deployment（デプロイメント）**

ReplicaSetの作成時に必要な機能。

Deploymentはまた、複数のReplicaSetを管理しデプロイを管理するリソースとして知られています。

## **Service (サービス)**

Serviceの役割とは、散らばっている**ポッド(Pod) にエンドポイントを割り与えIPアドレスを指定して通信したり、Podに対する負荷分散する**。

サービス無しではPodにアクセスすることが不可能となる。

## **Ingress**

**クラスター外から内部サービスへのHTTPとHTTPSのルートを公開するリソース**。

また、仮想ホスティングやSSL終端等などの機能の提供を行う役割があります。

# オブジェクトの作成方法

---

Kubernetesでオブジェクトを作成する場合、オブジェクトの基本的な情報(例えば名前)と共に、望ましい状態を記述したオブジェクトのspecを渡さないといけない。

## Deploymentについて

---

### ymlファイルの書き方

- `deployment.yml`サンプルソース
    
    ```
    apiVersion: apps/v1 # for versions before 1.9.0 use apps/v1beta2
    kind: Deployment
    metadata:
      name: nginx-deployment
    spec:
      selector:
        matchLabels:
          app: nginx
      replicas: 2 # tells deployment to run 2 pods matching the template
      template:
        metadata:
          labels:
            app: nginx
        spec:
          containers:
          - name: nginx
            image: nginx:1.14.2
            ports:
            - containerPort: 80
    ```
    

`.yaml`ファイルを利用してDeploymentを作成するには、`kubectl apply`コマンドに`.yaml`ファイルを引数に指定し、実行します。

例）

```
kubectl apply -f https://k8s.io/examples/application/deployment.yaml --record
```