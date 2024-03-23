---
html: ammwithdraw.html
parent: transaction-types.html
seo:
    description: LPトークを自動マーケットメーカーに返却し、プールが保有する資産の一部と引き換えマス。
labels:
  - AMM
---
# AMMWithdraw
[[ソース]](https://github.com/XRPLF/rippled/blob/master/src/ripple/app/tx/impl/AMMWithdraw.cpp "Source")

_([AMM amendment][])_

AMMの流動性プロバイダトークン（LPトークン）を返却することで、[自動マーケットメーカー](../../../../concepts/tokens/decentralized-exchange/automated-market-makers.md)(AMM)インスタンスから資産を引き出します。

## {% $frontmatter.seo.title %} JSONの例

```json
{
    "Account" : "rJVUeRqDFNs2xqA7ncVE6ZoAhPUoaJJSQm",
    "Amount" : {
        "currency" : "TST",
        "issuer" : "rP9jPyP5kyvFRb6ZiRghAGw5u8SGAmU4bd",
        "value" : "5"
    },
    "Amount2" : "50000000",
    "Asset" : {
        "currency" : "TST",
        "issuer" : "rP9jPyP5kyvFRb6ZiRghAGw5u8SGAmU4bd"
    },
    "Asset2" : {
        "currency" : "XRP"
    },
    "Fee" : "10",
    "Flags" : 1048576,
    "Sequence" : 10,
    "TransactionType" : "AMMWithdraw"
}
```

{% raw-partial file="/@i18n/ja/docs/_snippets/tx-fields-intro.md" /%}

| フィールド     | JSONの型   | [内部の型][] | 必須? | 説明         |
|:-------------|:-----------|:-----------|:------|:------------|
| `Asset`      | オブジェクト | STIssue    | はい   | AMMのプールにある資産の一つを定義します。JSONでは、`currency`と`issuer`フィールドを持つオブジェクトになります（XRPの場合は`issuer`を省略します）。 |
| `Asset2`     | オブジェクト | STIssue    | はい   | AMMのプールにあるもう一つの資産を定義します。JSONでは、`currency`と`issuer`フィールドを持つオブジェクトです(XRPの場合は`issuer`を省略)。|
| `Amount`     | [通貨額][]  | Amount     | いいえ | AMMから引き出す1つの資産の量。これは、AMMのプールにある資産の1つ（トークンまたはXRP）と一致する必要があります。 |
| `Amount2`    | [通貨額][]  | Amount     | いいえ | AMMから引き出す他の資産の量。存在する場合、これはAMMのプール内の他の資産と一致する必要があり、`Amount`と同じにすることはできません。 |
| `EPrice`     | [通貨額][]  | Amount     | いいえ | 引き出しに必要な、資産の1単位あたりに支払う最低有効価格（LPトークンの返却単位）。 |
| `LPTokenIn`  | [通貨額][]  | Amount     | いいえ | AMMのLPトークンの引き替え数。 |

**注記:** ダブルアセット出金の場合、`Asset1`と`Amount1`または`Amount2`が対応していれば、`Asset2`はもう一方に対応することが可能です。しかし、両者を一致させることをお勧めします(つまり、`Amount2`は`Asset2`で定義されたアセットの金額です)。その方が混乱を招きにくくなります。

### AMMWithdrawモード

このトランザクションには、指定するフラグによっていくつかのモードがあります。それぞれのモードは、特定のフィールドの組み合わせを必要とし、以下の2つのカテゴリに分類されます。

- **ダブルアセット出金**: AMMのプールから両方の資産をその残高と同じ割合で受け取ります。この出金には手数料はかかりません。
- **シングルアセット出金**: AMMのプールから1つの資産を受け取ります。AMMは、出金によりプール内の資産残高がどれだけ変動するかによって手数料を算出します。出金のモードによって、手数料が支払われたLPトークンの量から差し引かれるか、出金する資産の量から差し引かれるか決まります。

以下の項目の組み合わせは、**ダブルアセット出金**について示しています。

| フラグ名         | フラグ値       | 指定フィール         | 意味     |
|-----------------|--------------|---------------- ----|---------|
| `tfLPToken`     | `0x00010000` | `LPTokenIn`のみ     | 指定された量のLPトークンを返却し、LPトークンの発行総数に対する返却されたトークンの割合に基づく金額の両方の資産を受け取ります。 |
| `tfWithdrawAll` | `0x00020000` | なし                | LPトークンを _全て_ 返却し、AMMのプールにある両方の資産を最大限受け取ります。 |
| `tfTwoAsset`    | `0x00100000` | `Amount`と`Amount2` | 指定した金額を上限として、AMMの資産の両方を出金します。実際に受け取る金額は、AMMのプールの資産残高の割合と同じになります。 |

以下の項目の組み合わせは、**シングルアセット出金**について示しています。

| フラグ名                 | フラグ値      | 指定フィールド           | 意味     |
|-------------------------|--------------|-----------------------|---------|
| `tfSingleAsset`         | `0x00080000` | `Amount`のみ           | LPトークンを指定した数だけ返却し、1つの資産を指定した量だけ出金します。 |
| `tfOneAssetWithdrawAll` | `0x00040000` | `Amount`のみ           | LPトークンを全て返却することで、1つの資産を指定した金額以上出金します。指定された金額以上を受け取ることができない場合は失敗します。指定する金額は0でもかまいません。この場合、少しでも正の金額を出金できれば成功します。 |
| `tfOneAssetLPToken`     | `0x00200000` | `Amount`と`LPTokenIn` | 指定した量のLPトークンを返却することで、1つの資産を指定した量まで出金します。 |
| `tfLimitLPToken`        | `0x00400000` | `Amount`と`EPrice`    | 指定した1つの資産の量を上限として出金しますが、受け取る資産の一単位あたりのLPトークンで指定した有効価格より高い金額を支払うことはありません。 |

これら以外のフィールドとフラグの組み合わせは無効です。

### シングルアセット出金手数料

シングルアセット出金にかかる手数料は、ダブルアセット出金を行い、AMMを使用して指定しない方の資産を全て交換する場合と同じになるように計算されます。取引手数料は、交換に必要な金額に適用されますが、残りの出金分には適用されません。

<!-- TODO: add a formula and example calculation(s) of single-asset withdrawal fees -->

### AMMの削除

トランザクションがAMMに存在する全ての資産を出金すると、AMMは関連するすべてのトラストラインとともに自動的に削除されます。ただし、1回のトランザクションで削除できるトラストラインの数には制限があります。トラストラインが多すぎる場合、AMMは空の状態でレジャーに残ります。これは[AMMDelete トランザクション][]で削除するか、「空のAMM」に対する特別なダブルアセット入金([AMMDeposit トランザクション][])で補充することができます。AMMが空の間は、そのAMMに対する他の操作は無効です。

### AMMWithdrawのフラグ

AMMWithdrawトランザクションは、以下のように[`Flags`フィールド](../common-fields.md#flagsフィールド)の値をサポートしています。

| フラグ名                 | 16進数値      | 10進数値       | 説明                   |
|:------------------------|:-------------|:--------------|:----------------------|
| `tfLPToken`             | `0x00010000` | 65536         | 指定された額のLPトークンを返還する、ダブルアセット出金を行います。 |
| `tfWithdrawAll`         | `0x00020000` | 131072        | LPトークンをすべて返還する、ダブルアセット出金を行います。 |
| `tfOneAssetWithdrawAll` | `0x00040000` | 262144        | 全てのLPトークンを返還する、シングルアセット出金を行います。 |
| `tfSingleAsset`         | `0x00080000` | 524288        | 引き出す資産を指定して、シングルアセット出金を行います。|
| `tfTwoAsset`            | `0x00100000` | 1048576       | 両資産の金額を指定して、ダブルアセット出金を行います。 |
| `tfOneAssetLPToken`     | `0x00200000` | 2097152       | シングルアセット出金を行い、指定された額のLPトークンを受け取ります。 |
| `tfLimitLPToken`        | `0x00400000` | 4194304       | 有効価格を指定して、シングルアセット出金を行います。 |

これらのフラグのうちの **1つのみ** と、任意の[グローバルフラグ](../common-fields.md#グローバルフラグ)を指定する必要があります。


## エラーケース

すべてのトランザクションで発生する可能性のあるエラーに加えて、{% $frontmatter.seo.title %}トランザクションでは、次の[トランザクション結果コード](../transaction-results/index.md)が発生する可能性があります。

| エラーコード               | 説明                                          |
|:-------------------------|:---------------------------------------------|
| `tecAMM_EMPTY`           | AMMのプールに資産がありません。この状態では、AMMを削除するか、新しい入金を行い資金を供給することしかできません。 |
| `tecAMM_BALANCE`         | トランザクションによって、プールから1つの資産をすべて引き出そうとしている、もしくは`tfWithdrawAll`の場合に端数処理によって0以外の金額が残ってしまっています。 |
| `tecAMM_FAILED` | 例えば、`EPrice`フィールドに指定された有効価格が低過ぎる場合など、出金に関する条件が成立しませんでした。 |
| `tecAMM_INVALID_TOKENS`  | トークンペアのAMMが存在しないか、計算の結果、引き出し額がゼロに丸められました。 |
| `tecFROZEN`              | トランザクションは[凍結](../../../../concepts/tokens/fungible-tokens/freezes.md)されたトークンを引き出そうとしました。 |
| `tecINSUF_RESERVE_LINE`  | トランザクションの送信者は、このトランザクションを処理するための[準備金要件](../../../../concepts/accounts/reserves.md)の増加に対応できません。おそらく、引き出される資産の1つを保持するために少なくとも1つの新しいトラストラインが必要ですが、新しいトラストラインのための追加の所有者準備金分のXRPを持っていないためでしょう。 |
| `tecNO_AUTH`             | 送信者は、引き出し資産のいずれかを保有する権限を有していません。 |
| `temMALFORMED`     | トランザクションで無効なフィールドの組み合わせが指定されました。[AMMWithdrawモード](#ammwithdrawモード)を参照してください。 |
| `temBAD_AMM_TOKENS`      | 例えば、`issuer`がAMMの関連するAccountRootアドレスでない、`currency`がこのAMMのLPトークンの通貨コードでない、またはトランザクションがこのAMMのLPトークンをAssetフィールドの1つに指定した、などです。 |
| `terNO_AMM`              | トランザクションで指定した資産ペアの自動マーケットメーカーインスタンスが存在しません。 |

{% raw-partial file="/docs/_snippets/common-links.md" /%}