# Well-Architected Framework

1. Well-Architected Framework ホワイトペーパー
2. AWSのSAや認定パートナーによる支援制度
3. セルフチェック向けのWell-Architected Tool

## 5つの設計原則

- Reliability 信頼性
  - 基盤 IAM, VPC, ELB
  - 変更管理 CloudTrail
  - 障害管理 Cloud Watch
- Perfomance Efficiency パフォーマンス効率化
  - コンピューティング Auto Scaling, Lambda
  - ストレージ EBS, S3, Glacier
  - データベース RDS, Dynamo DB, Aurora
  - 容量と時間のトレードオフ Cloud Front
- Security 安全性
  - データ保護 ELB, ELS, RDS
  - 権限管理 IAM, MFA
  - インフラ保護 VPC
  - 検出制御 Cloud Trail, AWS GuardDuty
- Cost Optimization
  - 需要と供給の一致 Auto Scaling
  - コスト効率の高いリソース Trusted Advisor
  - 支出の認識
  - 支出の認識 Cloud Watch
  - 継続した最適化
- Operational Excellence 運用上の優秀性
  - 準備 Cloud Formation, Runbook Playbook
  - 運用 System Manager, Cloud Trail, AWS Config
  - 進化

## AWSベストプラクティス

11の原則を定義している

- スケーラビリティの確保
  - 需要の変化に対応できるアーキテクチャ
  - RDS, EC2 Auto Scaling, DynamoDB
- 環境の自動化
  - 主要プロセスを自動化する
  - Cloud Formation, ECS, Elastic Beanstalk
- 使い捨てリソースの使用
  - サーバーなどのコンポーネントを一時的なリソースとして利用
  - EC2, Auto Scaling
- コンポーネントの疎結合
  - コンポーネント間の相互依存を減らすことで、変更、障害の影響を削減
  - ELB, SNS, SQS
- サーバーではなくサービス (サーバレス)
  - 効率的な設計と運用
  - Lambda, ELB, SNS, SES
- 最適なデータベース選択
  - 目的別に選択する
  - RedShift, RDS, DynamoDB, Aurora, Elastic search
- 増大するデータ量対応
  - ビッグデータ時代に保持の効率化
  - S3, Kinesis
- 単一障害店の排除
  - 高可用設計が必要なサービスへのELB対応
  - EC2, Direct Connect, RDS
- コスト最適化
  - 5つの設計原則と同じ
- キャッシュの利用
  - 繰り返しデータなどについては、効率化
  - Cloud Front, Elastic Cache
- セキュリティの確保
  - 5つの設計原則と同じ
