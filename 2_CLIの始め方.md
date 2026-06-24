# 作業の始め方

ここでは、Azure CLI または Azure PowerShell を利用してAzureリソースを操作する際の基本的な流れと、作業前に確認すべきポイントについて説明します。

CLIによる操作は、GUI操作と比較して再現性が高く、作業履歴を残しやすいというメリットがあります。一方で、誤ったサブスクリプションや環境に接続した状態で操作を実行すると、意図しないリソースへの影響につながる可能性があります。

そのため、作業開始時には以下の流れを習慣化することが重要です。

---

# Azure CLI または Azure PowerShell を利用できるようにする

CLI操作を行うためには、Azure CLI または Azure PowerShell を利用できる環境を準備する必要があります。

## ローカル環境にモジュールをインストールして使用する

普段の運用作業や構築作業では、ローカル環境にモジュールをインストールして利用する方法が一般的です。

| 項目 | Azure CLI | Azure PowerShell |
|---|---|---|
| インストール方法 | https://learn.microsoft.com/ja-jp/cli/azure/install-azure-cli-windows?view=azure-cli-latest&pivots=msi | https://learn.microsoft.com/ja-jp/powershell/azure/install-azps-windows?view=azps-15.3.0&tabs=windowspowershell&pivots=windows-msi |

インストール後は、PowerShellからAzure操作用コマンドを実行できるようになります。

---

## ローカル環境にインストールせず利用する

Azure Portalから利用できる **Azure Cloud Shell** には、Azure CLI と Azure PowerShell の両方があらかじめインストールされています。

そのため、ローカル環境へのインストールは不要であり、ブラウザのみですぐに利用を開始できます。

初めてCLIを学習する場合や、一時的な検証を行う場合はCloud Shellの利用がおすすめです。

![alt text](<スクリーンショット 2026-06-24 085941.png>)

### Cloud Shell利用時の注意点

Cloud Shellは一定時間操作が行われない状態が続くとセッションが切断されます。

そのため、

- 実行履歴が失われる可能性がある
- 作業ログを証跡として残しづらい
- 長時間の構築作業には不向き

といったデメリットがあります。

実際の構築作業や運用作業では、作業記録を残しやすいローカル環境を利用することを推奨します。

---

# PowerShellを起動して実行ログを取得する

CLIによる作業では、実行したコマンドや結果をログとして残しておくことが重要です。

ログを取得しておくことで、

- 作業エビデンスとして利用できる
- 障害発生時の調査に利用できる
- 作業内容のレビューや再利用が可能になる
- 保存したログが取り出せない時がある

といったメリットがあります。

そのため、作業開始前にPowerShellのログ取得設定を行うことを推奨します。

ログの記録開始　※現在のディレクトリ(フォルダ)階層において、ログファイルを作成します。

```
Start-Transcript -Path ("$pwd\{0}.log)" -f (Get-Date -Format "yyyyMMddHHmm")
```

ログの記録停止

```
Stop-Transcript
```

---

# Azureへログインする

Azureリソースを操作するためには、まずAzureへ認証を行います。
PowerShellで以下を実行します。

| 項目 | Azure CLI | Azure PowerShell |
|---|---|---|
| コマンド | ``` az login ``` | ``` Connect-AzAccount ``` |

実行後、ブラウザが起動し、Azureアカウントによる認証が求められます。

認証が完了すると、Azure PowerShellからAzureリソースへアクセスできるようになります。

---

# 現在接続しているサブスクリプションを確認する

Azureのリソース操作は、基本的にサブスクリプション単位で実施されます。

例えば、

- サブスクリプションAのリソースグループに仮想マシンを作成したい
- サブスクリプションAのVNetへ接続したい

にもかかわらず、現在の接続先がサブスクリプションBであった場合、

- リソースグループが見つからない
- VNetが見つからない
- 誤った環境へリソースを作成してしまう

といった問題が発生する可能性があります。

そのため、作業開始時には必ず現在のサブスクリプションを確認してください。

---

## 現在のサブスクリプション情報を取得する

以下のコマンドで現在接続しているサブスクリプションを確認できます。

```powershell
Get-AzContext
```

実行例

```text
Name              : Subscription-A
Account           : user@example.com
SubscriptionName  : Subscription-A
SubscriptionId    : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
TenantId          : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
Environment       : AzureCloud
```

特に以下の項目を確認してください。

- SubscriptionName
- SubscriptionId

---

## 目的のサブスクリプションへ切り替える

現在のサブスクリプションが目的の環境でない場合は、作業対象のサブスクリプションへ切り替えます。

```powershell
Set-AzContext -Subscription "<サブスクリプション名>"
```

例

```powershell
Set-AzContext -Subscription "Production-Subscription"
```

または、

```powershell
Set-AzContext -Subscription "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
```

切り替え後は再度 `Get-AzContext` を実行し、意図したサブスクリプションに接続されていることを確認してください。

---

# ここから作業開始

以下の状態になっていれば、Azure CLI または Azure PowerShell を利用した作業を開始できます。

- Azureへログイン済み
- 実行ログを取得している
- 作業対象のサブスクリプションを確認済み
- 必要に応じてサブスクリプションを切り替え済み

これらの確認を実施することで、誤操作による影響を防ぎ、安全かつ再現性の高い作業を実施できます。