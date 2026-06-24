# AzureのCLI操作について

## 概要

Azureで仮想マシンやネットワーク、ストレージなどのリソースを構築する際、多くの場合はAzure Portalを利用して画面操作で設定を行います。<br>
Azure Portalは直感的に操作できるため非常に便利ですが、以下のような課題もあります。

- 手作業による設定ミスが発生する可能性がある
- 同じ作業を複数回実施する際に工数がかかる
- 作業者によって設定内容にばらつきが生じる
- 実施した操作内容を正確に残しづらい
- 大量のリソースを管理する際に効率が悪い

特に、複数環境への展開や定期的な運用作業を行う場合、***「毎回Portalから手作業で実施する方法で本当に品質を担保できるのか」、「もっと効率的な方法はないのか」***という課題に直面します。<br>
その解決策の一つが、CLI（Command Line Interface）を利用したAzure操作です。


## CLIとは

CLI（Command Line Interface）とは、***コマンド（文字列）を入力してシステムやサービスを操作する仕組み***のことです。

GUI（画面をクリックして操作する方法）と比較すると、以下のようなメリットがあります。

- 操作内容をコードとして残せる
- 同じ作業を何度でも同じ手順で実行できる
- 手作業による設定ミスを削減できる
- 作業の自動化が容易になる
- 大量のリソースを効率的に管理できる

Azureでは、仮想マシン、ネットワーク、ストレージ、データベースなど、ほぼすべてのリソースをCLIから操作できます。<br>
また、実際に実行したコマンドを手順書やスクリプトとして管理できるため、作業の再現性や品質向上にもつながります。

Azure CLI と Azure PowerShell の違い

| 項目 | Azure CLI | Azure PowerShell |
|---|---|---|
| ベース | Linux系コマンド風 | PowerShell |
| 対応OS | Windows / macOS / Linux | 主にWindows（他OSも可） |
| 基本文法 | az <リソース種別> <動詞> | <動詞>-Az<リソース種別> |
| 主力形式 | JSON | オブジェクト |

一般的には、

- OSを問わず利用したい、CI/CDと連携したい → Azure CLI
- PowerShellに慣れている、Windows運用が中心 → Azure PowerShell

という選択が多くなります。

また、Azure Automationなどの自動化サービスでは、Azure PowerShellとの親和性が高く、運用自動化の場面でよく利用されています。

## どんな時に使う？

例えば、

- 仮想マシンを複数台まとめて作成したい
- 開発環境と本番環境に同じ構成を展開したい
- 月次運用作業を自動化したい
- 実施した作業内容を記録として残したい

といったケースでは、Portal操作よりもCLIの方が高い効率と再現性を実現できます。

## どうやったら使えるようになる？

### ローカル環境にモジュールをインストールして使用

Azure CLIやAzure PowerShellを利用するためには、以下のいずれかの方法で実行環境を準備します。

| 項目 | Azure CLI | Azure PowerShell |
|---|---|---|
| インストール方法 | https://learn.microsoft.com/ja-jp/cli/azure/install-azure-cli-windows?view=azure-cli-latest&pivots=msi | https://learn.microsoft.com/ja-jp/powershell/azure/install-azps-windows?view=azps-15.3.0&tabs=windowspowershell&pivots=windows-msi|

### ローカル環境にインストールしないで使用

Azure Portalから利用できる***Azure Cloud Shell***には、Azure CLIとAzure PowerShellの両方があらかじめインストールされています。

そのため、ローカル環境へのインストールは不要で、ブラウザさえあればすぐに利用を開始できます。<br>
初めてCLIを学習する場合は、環境構築が不要なCloud Shellから始めるのがおすすめです。

![alt text](<スクリーンショット 2026-06-24 085941.png>)

ただし、これは20分以上経過するとセッションが消えてしまうため、実際の現場作業では記録が消えてしまうリスクがあります。<br>
したがって、環境ではローカル環境を使用したほうがいいと考えます。