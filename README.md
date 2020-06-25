# openshift-pipeline-practice-java-manifest
以下のレポジトリで生成したアプリケーションイメージをデプロイするためのマニフェストレポジトリ。
モノレポジトリの方針でマニフェストファイルも含むが、ソースコードレポジトリとマニフェストレポジトリを分離することのメリットをお伝えするために本レポジトリを作成しました。

https://github.com/mosuke5/openshift-pipeline-practice-java

## Argo CD
トップディレクトリには、OpenShift Template形式のマニフェストでJenkinsパイプラインを使ったデプロイするためのファイルが置いていますが、現代のKubernetes環境ではGitOpsの考え方は非常に重要な存在となっています。
Argo CDを使ったGitOpsの体験も本レポジトリで行うことが可能です。
ハンズオン資料は下記からご参照ください。

[Argo CDを使ったGitOpsの体験](argocd/argocd-ws.md)