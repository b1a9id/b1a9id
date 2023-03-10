# STORES株式会社（2018/06 ~ ）
雇用形態は正社員。Webアプリケーションエンジニアとして、STORES決済の開発に携わる。

## 2021/01 ~ 2021/04 JenkinsからGitHub Actionsへの移行

**利用技術：** AWS、Terraform、GitHub Actions

次のような理由で、JenkinsからGitHub Actionsへの移行を決めた。

1. デプロイ・リリース方法が多様化されている上にちゃんとドキュメントがなく、全環境の手順把握してる人もいないため、使うツールや手順を統一したい
2. 数年Jenkinsのメンテナンスされてこなかったし、これからもメンテナンスしたくない
3. ジョブの作り上、ビルド・デプロイに時間がかかるため改善したい

既存で使われていたのが、AWS CodeBuild・AWS CodeDeployとGitHub Actionsだったのでこの2択だった。前者だと、設定ファイル結構用意しないといけないしデプロイ作業が手間そうだったので、後者に決めた。
運用のことを考えて、実行ログの永続化のために独自でバッチを作ったり、Slack通知をするジョブを全レポジトリに展開するために独自アクションを作ったりした。

詳細は、[JenkinsからGitHub Actionsへの移行をキメた](https://www.b1a9idps.com/posts/migrate-to-github-actions)にまとめてある。

## 2020/12 ~ 2021/02 Elastic Beanstalkで動いているAPIのECS移行

**利用技術：** Java 1.8、Spring Boot 2.2.x、Spring Cloud Hoxton.RELEASE、AWS(IAM、S3、ALB、ECS、Security Group) 、Terraform、GitHub Actions

「2018/08 ~ 2019/03 コールセンターとカスタマーサポートチーム間の業務改善」のプロジェクトで作ったAPIがElastic Beanstalkで動いているのでECSに移行した。
「時代にそぐわない、Java 8より後のバージョンがAmazon Correttoしか利用できない、Beanstalkはブラックボックス化しているため不具合の原因調査等が難しい」など理由からECSへの移行を決めた。
デフォルトだと、アプリケーションログ、アクセスログ、エラーログが１箇所に吐き出されてしまうため、AWS FireLensを利用して分けて吐き出すようにした。デプロイはGitHub Actionsから行うようにした。

## 2020/12 ~ 2021/01 GitHub ActionsのデプロイログをS3にアップロードするバッチ作成

**利用技術：** Java 11、Spring Boot 2.4.x、Spring Cloud 2020.0.x（Spring Cloud Openfeign）、AWS(IAM、S3) 、Terraform、GitHub Actions

CI/CDをJenkinsからGitHub Actionsに移行するプロジェクトの前段階。監査の観点でデプロイログを一定期間保存する必要があるが、現在のGitHubのプランでは90日しか保存されないため、S3にデプロイログをアップロードするバッチを作成。
バッチは、「GitHubのAPIを叩いて、全レポジトリのワークフローの実行ログファイル（zip）を取得してS3にアップロードする」もので、これをGitHub Actionsのscheduleイベントを使って毎日12時に起動するようにした。
LocalStackを使ってユニットテストを書きたかったが、AWS SDK for Java v2は対応してないため断念。

## 2020/08 ~ 2020/12 社内用管理システムのユーザ管理・権限管理機能追加

**利用技術：** Java 1.8、Spring Boot 2.3.x、AWS(Elastic Beanstalk、Cloud Watch、Parameter Store、Amazon Aurora) 、Terraform

全加盟店を管理している社内用管理システム(S)では、Sシステム利用ユーザの登録や参照はできず（リプレース前の旧システムBで行っていた）。権限管理機能もなかった。Bシステムの停止や誤操作の防止のために機能追加を行った。
社内ユーザ管理や認証認可に関するところは、ローンチ時からほぼ手をつけられてなかったし今後もあまりタッチできないだろうと思い、大規模なリファクタリングも行った。具体的にはレイヤードアーキテクチャ、マイクロサービスなどの現在の実装方針に合うように実装した。
DB移行も必要だったため、TerraformでRDSを構築した。

また、社内にまだ実績のなかった、[springdoc-openapi](https://github.com/springdoc/springdoc-openapi)や[Spring Cloud OpenFeign](https://github.com/spring-cloud/spring-cloud-openfeign)やAWS Systems Manager パラメータストアを導入した。springdoc-openapiではドキュメントとコードの乖離の防止、Spring Cloud OpenFeignではHttpClientの実装工数削減、AWS Systems Manager パラメータストアではセキュリティレベルの向上ができた。
検証時の様子をブログにまとめてある。

- [springdoc-openapiでOpenAPI形式のAPIドキュメントを生成する](https://www.b1a9idps.com/posts/springdoc-openapi-1)
- [Spring Cloud OpenFeignで遊ぶ](https://www.b1a9idps.com/posts/spring-cloud-open-feign-1)
- [Spring BootアプリケーションでAWS Systems Manager パラメータストアを利用する](https://www.b1a9idps.com/posts/spring-boot-parameter-store)

m要件外のリファクタリングの変更が多かったが、バグを出してしまっては意味がないので、テスト項目書を作成しフロントエンドチームとQA前に2週間かけてテストを行った。QA期間中は疑問点等を自ら拾って回答するようにして品質向上に協力した。
プロジェクト開始時に立てたスケジュールよりも前倒しで進められた上に、リリース後バグもなくとても満足のいくプロジェクトとなった。

## 2020/07 TravisからGitHub Actionsへの移行

**利用技術：** GitHub Actions

開発時のCIはTravisを利用していたが、同レポジトリで動くジョブば1つで複数人が同時にPUSHすると、ビルド待ちがけっこう発生していた（1ビルドあたり10分）のでGitHub Actionsに移行した。
社内にまだ知見がなかったので、ドキュメントを読み込み手を動かした。GitHub Actionsにしたことで、パラレルでジョブが走るために待ち時間が短縮できた。また、GitHubだけで完結するために設定が楽になった。

## 2020/01 ~ 2020/07 入金サイクルの短縮化

**利用技術：** Java 1.8、Spring Boot 2.2.x、AWS(Elastic Beanstalk、Cloud Watch)

競合サービスよりも加盟店さまへの入金までのスピードが遅かったため、2週間から翌々日に入金できるようにした。仕様検討、実装、テストを担当。
入金周りのコードはサービスローンチ時からほぼ手をいれおらず、実際の仕様を表現した実装になっていなかったり、特に方針もなく実装されていたので機能開発に加えて大規模リファクタリングも行った。
リリース直後から入金依頼が行われていて、加盟店さまにとってとても価値があることができたと実感できた。

## 2020/01 一次請けコールセンターの業務改善

**利用技術：** Java 1.8、Spring Boot 2.2.x、React、TypeScript

社内専用サービスは、コールセンターの方たちは参照しかできないように制限をかけていたことで、冗長な書き込み系の作業が発生していたため、この工数を削減した。仕様検討、実装（フロントエンド・バックエンド）、テストを担当。
バックエンドはアクセス制限の変更（一部POSTを可能に）を行い、フロントエンドは表示内容の変更を行った。

## 2019/05 ~ 2019/12 Spring Boot 1.5.xからSpring Boot 2.2.xへのバージョンアップ

**利用技術：** Java 1.8、Spring Boot 2.2.x、Spring Cloud Config Server、Gradle

5レポジトリをGradle 4.10.2以上、Spring Boot 2.2.xにしてリリースした。方針検討、実装、テストを担当。
リリースノートや公式ドキュメント、ソースコードを読みながらバージョンアップを行い、リファクタリングも同時に行った。
QAチームと一緒にテスト項目書を作ってテストをしたのもあって、特に大きな問題もなくリリースできた。

このときの様子を[note](https://note.com/b1a9idps/n/n0b9ca2ee57a2)にまとめている。

## 2019/04 認証・認可周りの改修

**利用技術：** Java 1.8、Spring Boot 1.5.x

Spring SecurityのSecurity Filter Chainを通過した後のSpring Webに処理が移ったときに認証・認可を行なっていた。Spring的に正しい実装方法ではなく、またサービス拡張を考えたときに拡張しづらくなることを考えて改修した。方針検討、実装、テストを担当。
Spring Securityを適切に利用することでセキュリティが高く、今後の拡張もしやすくなった。

## 2018/08 ~ 2019/03 コールセンターとカスタマーサポートチーム間の業務改善

**利用技術：** Java 1.8、Spring Boot 2.1.x、AWS(Elastic Beanstalk、Cloud Watch、Amazon Aurora、Cognito、S3)

コールセンターの方たちは社内専用サービスを利用することができず、スプレッドシート等でカスタマーサポートチームを必要な情報をやりとりしている状況だった。業務改善のために、社内専用サービスをコールセンターの方たちも利用できるようにした。API設計、実装、インフラ設計・構築、テスト担当。
当時社内に知見のなかったSpring Boot 2やMicrometer、TestContainersを導入しました。Amazon CognitoのSDKとSpring Security使って認証・認可処理を実装するのは特に苦戦した。TestContainersを導入したことで、本番と同じDBMSを利用したテストを書けるようになり、よりクオリティ高いテストを書けるようになった。
開発以外では、プロジェクト全体の進捗管理を行ったり、QAやビジネスサイドの人たち向けに噛み砕いた資料を作って仕様説明会を行ったりした。
