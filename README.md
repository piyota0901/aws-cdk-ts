# aws-cdk-ts
aws cdk for typescript

# 作業環境
- npm
- docker / docker-compose
- AWS CLI
- AWS CDK Local
  - https://github.com/localstack/aws-cdk-local
  1. `aws-cdk-local`をインストール
		```bash
		$ npm install aws-cdk-local

		added 2 packages, and audited 3 packages in 2s

		found 0 vulnerabilities
		```
	1. `aws-cdk`をインストール
		```bash
		$ npm install aws-cdk
		added 1 package, and audited 4 packages in 4s

		found 0 vulnerabilities
		```
- poetry
  - `Localstack CLI`インストール
	```bash
	$ poetry add localstack
	```
  - `awscli-local`インストール
	```bash
	$ pip install awscli-local
	```
# 前提条件
- CDK: Typescript
- Localstackの起動/停止: `Localstack CLI`
- aws cli: `awscli-local` -> `awslocal xx xxxx`

# CDKのセットアップ
1. ディレクトリ作成
   ```bash
   $ mkdir cdk-sample
   ```
1. cdkの初期化
   ```bash
   $ cd cdk-sample
   $ npx cdklocal init sample-app --language typescript
   ```
1. cdkの`bootstrap`
   ```bash
   $ npx cdklocal bootstrap
    ⏳  Bootstrapping environment aws://000000000000/us-east-1...
	Trusted accounts for deployment: (none)
	Trusted accounts for lookup: (none)
	Using default execution policy of 'arn:aws:iam::aws:policy/AdministratorAccess'. Pass '--cloudformation-execution-policies' to customize.
	CDKToolkit: creating CloudFormation changeset...
	✅  Environment aws://000000000000/us-east-1 bootstrapped.
   ```
1. サンプルのスタックのデプロイ
   ```bash
   $ npx cdklocal synth
   $ npx cdklocal deploy
   ✨  Synthesis time: 7.58s

	This deployment will make potentially sensitive changes according to your current security approval level (--require-approval broadening).
	Please confirm you intend to make the following modifications:

	IAM Statement Changes
	┌───┬───────────────────────┬────────┬─────────────────┬───────────────────────────┬───────────────────────────────────────────────────────┐
	│   │ Resource              │ Effect │ Action          │ Principal                 │ Condition                                             │
	├───┼───────────────────────┼────────┼─────────────────┼───────────────────────────┼───────────────────────────────────────────────────────┤
	│ + │ ${CdkSampleQueue.Arn} │ Allow  │ sqs:SendMessage │ Service:sns.amazonaws.com │ "ArnEquals": {                                        │
	│   │                       │        │                 │                           │   "aws:SourceArn": "${CdkSampleTopic}"                │
	│   │                       │        │                 │                           │ }                                                     │
	└───┴───────────────────────┴────────┴─────────────────┴───────────────────────────┴───────────────────────────────────────────────────────┘
	(NOTE: There may be security-related changes not in this list. See https://github.com/aws/aws-cdk/issues/1299)

	Do you wish to deploy these changes (y/n)? y
	```
1. localstackの状態確認
   - `bootstrap`のバケットが作成されていることを確認
		```bash
		$ awslocal s3 ls
		2022-08-21 15:33:30 test-aws-local-cli
		2022-08-21 15:46:29 cdk-hnb659fds-assets-000000000000-us-east-1
		```
   - AWS SQSサービスでQueueが作成されていることを確認
		```bash
		$ awslocal sqs list-queues
		{
			"QueueUrls": [
				"http://localhost:4566/000000000000/CdkSampleStack-CdkSampleQueue5EE69D51-4ce02b21"
			]
		}
		```
1. サンプルスタックの削除
   	```bash
   	$ npx cdklocal destroy
	Are you sure you want to delete: CdkSampleStack (y/n)? y
	CdkSampleStack: destroying...

	✅  CdkSampleStack: destroyed
	```

# CDKでVPC作成



# その他
## Poetry
- poetryの仮想環境起動
  ```bash
  $ poetry shell
  ```

## Localstack
- localstackの起動
  ```bash 
  $ localstack start -d 
        __                     _______ __             __
       / /   ____  _________ _/ / ___// /_____ ______/ /__
	  / /   / __ \/ ___/ __ `/ /\__ \/ __/ __ `/ ___/ //_/
	 / /___/ /_/ / /__/ /_/ / /___/ / /_/ /_/ / /__/ ,<
	/_____/\____/\___/\__,_/_//____/\__/\__,_/\___/_/|_|

	💻 LocalStack CLI 1.0.4

	[15:29:55] starting LocalStack in Docker mode 🐳                                                                               localstack.py:138
			preparing environment                                                                                                bootstrap.py:667
			configuring container                                                                                                bootstrap.py:675
			starting container                                                                                                   bootstrap.py:681
	[15:30:09] detaching   
	```
- 起動確認(別のshellで実行)
  ```bash
  $ localstack status services
    ┏━━━━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━┓
	┃ Service                  ┃ Status      ┃
	┡━━━━━━━━━━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━┩
	│ acm                      │ ✔ available │
	│ apigateway               │ ✔ available │
	│ cloudformation           │ ✔ available │
	│ cloudwatch               │ ✔ available │
	│ config                   │ ✔ available │
	│ dynamodb                 │ ✔ available │
	│ dynamodbstreams          │ ✔ available │
	│ ec2                      │ ✔ available │
	│ es                       │ ✔ available │
	│ events                   │ ✔ available │
	│ firehose                 │ ✔ available │
	│ iam                      │ ✔ available │
	│ kinesis                  │ ✔ available │
	│ kms                      │ ✔ available │
	│ lambda                   │ ✔ available │
	│ logs                     │ ✔ available │
	│ opensearch               │ ✔ available │
	│ redshift                 │ ✔ available │
	│ resource-groups          │ ✔ available │
	│ resourcegroupstaggingapi │ ✔ available │
	│ route53                  │ ✔ available │
	│ route53resolver          │ ✔ available │
	│ s3                       │ ✔ available │
	│ s3control                │ ✔ available │
	│ secretsmanager           │ ✔ available │
	│ ses                      │ ✔ available │
	│ sns                      │ ✔ available │
	│ sqs                      │ ✔ available │
	│ ssm                      │ ✔ available │
	│ stepfunctions            │ ✔ available │
	│ sts                      │ ✔ available │
	│ support                  │ ✔ available │
	│ swf                      │ ✔ available │
	└──────────────────────────┴─────────────┘
  ```
## AWS CLI Local
- s3バケット作成
  ```bash
  $ awslocal s3 mb s3://test-aws-local-cli
  ```
  - 確認
  ```bash
  $ awslocal s3 ls
  2022-08-21 15:33:30 test-aws-local-cli
  ```



# トラブルシューティング
- `poetry add xxx`の`Resolving dependencies...`が異常に長い場合
  - IPv6の通信の利用をOFF
  - https://zenn.dev/mangolassi/scraps/17b7df8a62b3bf
  ```bash
  sudo sysctl -w net.ipv6.conf.all.disable_ipv6=1
  sudo sysctl -w net.ipv6.conf.default.disable_ipv6=1
  ```