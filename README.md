ユーザレコメンド機能つきショッピングサイトのAWSを用いた構成作成を思考実験として行ってみたものです。
データの流れは以下。
1.ユーザーアクセス： ユーザーがウェブサイトにアクセスすると、Route 53がDNS解決を行い、適切なエンドポイントにルーティングする。
2.エッジサービス処理：WAFがトラフィックをフィルタリングし、CloudFrontがコンテンツをキャッシュして配信を高速化。
3.ロードバランシング：Application Load Balancerが、トラフィックを複数のEC2インスタンスへと分散。
4.アプリケーション処理：EC2 Auto Scalingグループ内のインスタンスがユーザーリクエストを処理し、必要に応じてElastiCacheからデータを取得。
5.API処理： API Gatewayが、アプリケーションからのAPIリクエストを適切なバックエンドサービス（EMR,Lambda,DynamoDB）にルーティング。
6.データストリーム処理： Kinesis Data Streamsがリアルタイムのユーザーアクションデータを収集し、Lambdaがそのデータを処理。
7.データ保存：処理されたデータはDynamoDBに保存され、必要に応じてアプリケーションから参照される。
8.大規模データ処理： EMRが定期的に大量のデータを処理し、その結果をS3に保存する。
9.レコメンド:　Kinesisが収集したリアルタイムデータとEMRで分析されたデータに基づき、Lambdaがパーソナライズされた商品情報を生成。
API Gatewayを通じてアプリケーションに提供され、ユーザーへリアルタイムかつ最適なレコメンドを表示。
