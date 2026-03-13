# 複数台の仮想マシンを作成する方法

1台のみ作成する場合は、Azure Portalから作成する場合と手間は大きく変わりません。  

しかし、PowerShellで**繰り返し処理（For）**を使用することで、指定した台数分の仮想マシンを自動作成できます。

これにより、複数台作成する場合の作業効率が大幅に向上します。

---

## ■ 前提

- リソースグループ「sample-rg0」が作成済み  
- 仮想ネットワーク「sample-vnet」が作成済み  
- サブネット「default」が作成済み  

---

# 複数台の仮想マシンを作成する方法（末尾 -01 形式）

## ■ 複数台作成用コマンド

以下のスクリプトでは、指定した台数分の仮想マシンを  
`sample-vm-01` の形式で連番作成します。

```powershell
#=======================================
#変数定義（必要に応じてパラメータを変更）
#=======================================

$SubID = "Subscription-spoke-a" #サブスクリプションのID
$ResourceGroupName = "sample-rg" #リソースグループ名
$Location          = "japaneast" #リージョン

$VMCount           = 3   # 作成する仮想マシンの台数

# 既存VNet / Subnet
$VnetName   = "sample-vnet"
$SubnetName = "default"

# 管理者アカウント（ユーザー名、PWを定義。実行時に入力するため変更不要）
$Credential = Get-Credential

#=======================================
#メイン処理
#=======================================

# サブスクリプションを指定
Set-AzContext -Subscription $SubID

# 仮想マシンを複数作成
for ($i = 1; $i -le $VMCount; $i++) {

    # 2桁ゼロ埋め (-01, -02, -03)
    $Number = "{0:D2}" -f $i
    $VMName = "sample-vm-$Number"

    New-AzVM `
      -ResourceGroupName $ResourceGroupName `
      -Location $Location `
      -Name $VMName `
      -VirtualNetworkName $VnetName `
      -SubnetName $SubnetName `
      -Image "Win2022AzureEdition" `
      -Credential $Credential
}
```

---

## ■ 作成されるVM名の例

`$VMCount = 3` の場合

- sample-vm-01  
- sample-vm-02  
- sample-vm-03  

---

## ■ for文の構造

```powershell
for (初期値; 条件; 増減処理) {
    実行処理
}
```

今回の例では：

- 初期値：`$i = 1`
- 条件：`$i -le $VMCount`
- 増減処理：`$i++`

---

### 3. 管理者アカウントは1回のみ入力

```powershell
$Credential = Get-Credential
```

ループ外に記載することで、毎回入力する必要がありません。

---

## ■ 実行結果イメージ

`$VMCount = 3` の場合、以下の仮想マシンが作成されます。

| VM名        | サイズ             | OSイメージ |
|------------|-------------------|------------|
| sample-vm1 | Standard_D2s_v3   | Windows Server 2022 |
| sample-vm2 | Standard_D2s_v3   | Windows Server 2022 |
| sample-vm3 | Standard_D2s_v3   | Windows Server 2022 |

---

## ■ 注意事項

- 同時に複数作成するため、サブスクリプションのクォータ制限に注意してください。
- 作成には数分かかります。
- 本スクリプトは「とりあえずVMを作成したい方向け」の最小構成です。  

より詳細なパラメータ（可用性ゾーン、ディスク構成、NIC設定など）を指定したい場合は、  
以下の公式ドキュメントを参照し、`New-AzVM` コマンドに必要なパラメータを追加してください。

https://learn.microsoft.com/ja-jp/powershell/module/az.compute/new-azvm?view=azps-15.3.0#-image

---

## ■ まとめ

PowerShellの繰り返し処理を使用することで、

- Azure Portalで1台ずつ作成する手間を削減できる
- 検証環境や演習環境の一括構築が可能
- IaC（Infrastructure as Code）の第一歩になる

複数台構築を行う場合は、PowerShellによる自動化を推奨します。
