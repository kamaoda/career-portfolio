---
layout: default
title: 技術スタック
permalink: /skills/
---

# 技術スタック

Java・SpringとSQL・RDBを中心に、業務システムの開発に携わってきました。

特に得意としているのは、**業務要件をデータモデル・SQL・Javaの処理に落とし込み、一体として設計・実装すること**です。

バックエンド開発を軸に、API設計、テスト自動化、AWSを利用したアプリケーション構成、フロントエンド開発なども経験しています。技術方針の策定や設計・コードレビュー、業務改善にも取り組んできました。

## 技術スタック概要

| 領域 | 主な技術 | 経験・担当範囲 |
|---|---|---|
| **バックエンド** | Java、Spring Boot、Spring Batch、MyBatis、JPA / JPQL | アプリケーション設計・実装、共通処理の構築、設計・コードレビュー |
| **SQL・データベース** | PostgreSQL、Aurora PostgreSQL、Db2、Redshift | データモデル・SQLの設計・実装、大量データ処理、設計レビュー |
| API・認証認可 | REST API、OpenAPI、SOAP、Redis | API設計・実装、認証・認可、共通処理の構築 |
| テスト・品質保証 | JUnit、Mockito、DbUnit、LocalStack、Docker Compose | 自動テストの設計・実装、テスト環境構築、検証方針策定 |
| AWS | API Gateway、Lambda、Aurora PostgreSQL、Redshift、S3、Fargate、Glue、AWS CLI | アプリケーション構成・設定の設計、開発・検証 |
| フロントエンド | TypeScript、React、Next.js、Bootstrap | 業務画面の開発・改修、技術選定・レビュー、個人開発 |
| 開発支援・業務自動化 | シェルスクリプト、Excel関数、VBA、Python | 実行・検証の自動化、業務改善、試作 |

※技術ごとに担当範囲は異なります。以下では、設計・実装と、レビュー・試作・利用を中心とした経験を区別して記載します。

## Java・Spring

Javaを中心に、REST API、バッチ処理、外部システム連携の設計・実装を担当してきました。

Spring Boot・MyBatisを用いた開発では、プロジェクトの初期構成を整え、サンプルAPIや共通例外処理、個人情報のログ出力を制御する仕組みを実装しました。

Spring Batchを用いた開発では、ジョブ・Stepの構成設計からReader・Processor・Writerの主要処理まで実装しました。複雑な業務判断が必要な処理では、SQLによる対象データの抽出とJavaによる集約・加工を組み合わせました。

また、JPA・JPQLを利用した業務システムの開発経験もあります。

**主な使用バージョン（案件ごと）：** Java 8 / 11 / 17 / 21、Spring Boot 3系、Spring Framework 5系、Spring Batch 4系

**関連実績：** [複雑な保険契約変更に対応する外部連携バッチの設計・実装]({{ '/cases/insurance-contract-integration/' | relative_url }})

## SQL・データベース

データモデルの設計からSQLの実装、Java側の処理まで一貫して担当してきました。

### データモデル・SQLの設計と実装

損害保険関連システムでは、契約情報や外部連携の実績をDBに保持し、**その時点で必要な処理をSQLで導出する方式**を設計・実装しました。

入力パターンごとにJavaの条件分岐を増やすのではなく、データモデルとSQLによって複雑な業務判断を整理しました。

また、統計APIの開発では、データマートや集計・ランキング用SQLの設計・実装を担当しました。

### 大量データ処理・性能への対応

性能テスト用の大量データ生成では、アプリケーションからの逐次登録やCSV転送によるオーバーヘッドを避けるため、PostgreSQL上で直接データを生成・登録する方式を設計・実装しました。

`generate_series()`、`TEMPORARY TABLE`、`JOIN`、`CASE`式などを組み合わせ、関連テーブル間の整合性を維持しながら、**最大のテーブルで1億件を超えるテストデータを生成**しました。

### DB製品ごとの経験

- **PostgreSQL / Aurora PostgreSQL**：データモデル・SQLの設計・実装、大量データ生成、検証
- **Db2**：JPA・JPQLを利用したアプリケーション開発、検証用SQLの作成
- **Redshift**：カラム指向DBの特性や、主キー・一意制約が一意性を強制しない点などを考慮したテーブル設計レビュー

**関連実績：** [複雑な保険契約変更に対応する外部連携バッチの設計・実装]({{ '/cases/insurance-contract-integration/' | relative_url }})

## API設計・認証認可

REST APIの設計・実装、OpenAPIによるインタフェース定義方針の策定、SOAPによる外部システム連携を担当しました。

認証・認可では、Redisを利用したセッション管理や、Servlet Filter・Spring MVCのInterceptorを用いた認証・認可処理を設計・実装しました。アクセストークンの更新や、API・データ単位のアクセス制御にも対応しました。

また、HTTPエラーレスポンスの共通化や、個人情報をログに出力しないための仕組みも実装しました。

## テスト設計・自動化

JUnit 4・5とMockitoによるテストコードの作成に加え、DbUnit・LocalStack・Docker Composeなどを組み合わせたテスト環境を構築しました。

Spring Batchの開発では、JUnitから処理日付を変更しながら複数年分のバッチを連続実行し、DbUnitで最終的なDB状態を検証する仕組みを構築しました。外部連携はモックやLocalStackで代替し、ローカルで繰り返し検証できるようにしました。

AWS環境でのシナリオテストでは、AWS CLIとシェルスクリプトによる実行自動化に加え、検証対象と確認方法の見直しも担当しました。

**関連実績：**

- [複雑な保険契約変更に対応する外部連携バッチの設計・実装]({{ '/cases/insurance-contract-integration/' | relative_url }})
- [5年分の日次バッチ検証の効率化]({{ '/cases/daily-batch-validation/' | relative_url }})

## AWS・アプリケーション構成

API Gateway、Lambda、Aurora PostgreSQL、Glueなどを組み合わせたアプリケーション構成や、AWSリソースの設定内容を設計しました。

統計APIの開発では、Pythonによるデータマート更新処理の試作・検証や、本番コードのレビューも担当しました。AWSリソースの構築や一部の本番実装は別担当でした。

また、S3・Fargate・AWS CLIを利用したバッチ処理の実行自動化も経験しています。

AWSでは、**各種サービスを組み合わせたアプリケーションの構成設計・開発**を主な担当領域としています。

**関連実績：** [5年分の日次バッチ検証の効率化]({{ '/cases/daily-batch-validation/' | relative_url }})

## フロントエンド

TypeScript・React・Bootstrapを用いた業務画面の開発・改修を担当しました。

社内コーポレートサイトの刷新では、Next.jsの技術選定から設計・コードレビュー、実装・改修まで携わりました。

個人開発でもNext.js・React・TypeScriptを利用し、Prismaによるデータ管理を含むWebアプリケーションを開発しています。

## 開発支援・業務自動化

シェルスクリプトやAWS CLIによるバッチ実行の自動化、Excel関数・VBAによるテストデータや検証資料の生成など、反復作業を効率化する仕組みを開発しました。

社内の勤怠集計業務では、新規アプリケーションを開発する代わりに、既存SaaSの機能活用と業務ルールの見直しを提案しました。Excelによる自動計算の全体設計、実装方針の策定、レビューを担当しました。

**関連実績：** [既存の勤怠管理SaaSを活用した残業・有給計算の自動化]({{ '/cases/attendance-automation/' | relative_url }})

## 保有資格

- データベーススペシャリスト試験
- 統計検定1級（社会科学）
- Oracle Certified Java Programmer, Gold SE 8
- 応用情報技術者試験
- 基本情報技術者試験
- HTML5プロフェッショナル認定試験 レベル1