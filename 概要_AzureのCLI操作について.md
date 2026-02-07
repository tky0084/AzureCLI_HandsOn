# AzureのCLI操作について

AzureのCLI操作には、以下の2通りがあります。

- **Azure CLI**
- **Azure PowerShell**

どちらもコマンドラインからAzureリソースを操作するためのツールですが、  
利用するシェルやコマンド体系に違いがあります。

---

## CLIとは

CLI（Command Line Interface）とは、  
**コマンド（文字列）を入力してシステムやサービスを操作する仕組み** のことです。

GUI（画面をクリックして操作する方法）と比べて、以下の特徴があります。

- 操作を自動化しやすい
- 同じ作業を何度も正確に実行できる
- スクリプトとして管理・共有できる
- 大量のリソース操作に向いている

Azureでは、CLIを使うことで仮想マシン、ストレージ、ネットワークなどの  
さまざまなリソースを効率的に管理できます。

---

## Azure CLI と Azure PowerShell の違い

| 項目 | Azure CLI | Azure PowerShell |
|---|---|---|
| ベース | Linux系コマンド風 | PowerShell |
| 対応OS | Windows / macOS / Linux | 主にWindows（他OSも可） |
| 学習コスト | 比較的低い | PowerShellの知識が必要 |
| 主な用途 | クロスプラットフォーム、CI/CD | Windows管理、既存PS環境 |

一般的には、  
- **OSを問わず使いたい・CI/CDで使いたい → Azure CLI**
- **PowerShellに慣れている → Azure PowerShell**  
という選択が多いです。

---

## どんな時に使う？

Azure CLIは、以下のような場面でよく使われます。

- Azureリソースの作成・削除・設定変更
- 環境構築の自動化（IaCの一部として）
- 定型作業の効率化
- CI/CDパイプラインからのAzure操作
- GUIでは操作しづらい大量リソースの管理

例：
- 仮想マシンをまとめて作成・削除したい
- 環境ごと（開発・検証・本番）に同じ構成を作りたい
- 手順書をコマンドとして残したい

---

## どうやったら使えるようになる？始め方

Azure CLIを使い始めるまでの基本ステップです。

### 1. Azure CLIのインストール
以下のいずれかの環境にインストールします。

- ローカルPC（Windows / macOS / Linux）
- Azure Cloud Shell（ブラウザ上で利用可能）

※ Cloud Shellはインストール不要ですぐ使えます。

---

### 2. Azureにログイン
```bash
az login
