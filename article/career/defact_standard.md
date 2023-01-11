# 株式会社デファクトスタンダード（2018/11 ~ 2021/11 ）
雇用形態は、業務委託。Webアプリケーションエンジニアとして、Fashion Charity Projectの開発に携わる。

## 2020/10 HTTPClientをSpring Cloud OpenFeignに書き換え

**利用技術：** Java 1.8、Spring Boot 2.3.x、Spring Cloud OpenFeign(Spring Cloud Hoxton.SR8)
、PostgresSQL、Docker

サービスローンチ時に自前でHTTPClientを実装していたが、実装の簡単さと少なさを考えてSpring Cloud OpenFeignを利用するようにした。
Controllerを書く感覚でインターフェースを実装するだけでHTTPClientを生成してくれるのはかなり便利だなと感じた。
破壊的な変更だったため、1週間かけてサービス全体を動作確認したこともありリリース後まったく問題が発生しなかった。

## 2020/05 ~ 2020/06 断チャリプロジェクト

**利用技術：** Java 1.8、Spring Boot 2.2.x、HTML(Thymeleaf)、jQuery、PostgresSQL、Docker**URL：
** https://www.furusato-tax.jp/feature/a/fashion_charity_project

[ふるさとチョイス](https://www.furusato-tax.jp/?header) と連携して、自治体に寄附できるようにするようにした。内部設計、実装、テストを担当。
新型コロナウイルスに関連したプロジェクトであり、リリースまでのスピード感が必要だった。立ち上げからリリースまで3週間で行った。
リリース会見が行われるため通常の寄付よりも流入が大きくそうだったので、これまで以上にパフォーマンスを意識して実装をした。特に大きな問題は発生しなかった。

## 2020/04 ブランドページのリニューアル

**利用技術：** Java 1.8、Spring Boot 2.2.x、HTML(Thymeleaf)、jQuery、PostgresSQL、Docker

[ブランド一覧ベージ](https://www.waja.co.jp/fcp/brands) をブランディアで取り扱っているブランドの一覧に変更。内部設計、実装、テストを担当。
ブランド一覧の変更や月1で更新するためのCSVアップロード機能の実装を行った。月1しか更新されないため、キャッシュの実装も行った。

## 2019/01 認証認可の実装リファクタリング

**利用技術：** Java 1.8、Spring Boot 2.0.2、HTML(Thymeleaf)、jQuery、PostgresSQL、Docker

Spring Security的に適切なクラスで適切な処理を書いていなかったため、リファクタリングを行った。方針検討、実装、テストを担当。
また、外部APIへの無駄なリクエストがあったり、セッションで保存しているログイン情報の取り扱いミスがあったりしたので、同時にリファクタリングも行った。

## 2018/12 ~ 2019/05 マイページ実装

**利用技術：** Java 1.8、Spring Boot 2.0.2、HTML(Thymeleaf)、jQuery、PostgresSQL、Docker

寄付者が自分の寄付実績を確認できるようにマイページを実装した。内部設計、実装、テストを担当。
複数のテーブルを結合して実績を出すため、Criteria APIを使ってSQLを組み立てると複雑になりすぎることを懸念して、Native
Queryを書くことにした。テスト項目が多かったこと、ヘッダのリプレースも行ったこともあり当初のスケジュールより遅めのリリースになってしまった。リリース後は、クリティカルなバグは出なかった。
