---
html: multi-signing.html
parent: accounts.html
blurb: マルチシグを使用することで、トランザクション送信時のセキュリティが強化されます。
labels:
  - スマートコントラクト
  - セキュリティ
---
# マルチシグ

マルチシグは、複数のシークレットキーを組み合わせて使用してXRP Ledgerの[トランザクションを承認する](transactions.html#トランザクションの承認)手法です。アドレスで有効な承認手法（マルチシグ、[マスターキーペア](cryptographic-keys.html#マスターキーペア)、[レギュラーキーペア](cryptographic-keys.html#レギュラーキーペア)など）を自由に組み合わせて使用できます。（唯一の要件は、 _少なくとも1つの_ 手法を有効にする必要があることです。）

マルチシグには次のメリットがあります。

* 複数のデバイスからのキーを要求できます。これにより、不正使用者があなたの代わりにトランザクションを送信するには複数のマシンを悪用しなければならなくなります。
* 複数のユーザー間で1つのアドレスの管理を共有できます。この場合、各ユーザーが、そのアドレスからトランザクションを送信する際に必要な複数のキーのいずれか1つだけを所有します。
* あなたのアドレスからトランザクションを送信できる権限を、複数ユーザーのグループに委任できます。委任を受けた各ユーザーは、あなたが通常の方法で署名できない場合にあなたのアドレスを制御できます。
* その他のメリットもあります。

## 署名者リスト

マルチシグを使用するには、あなたの代理として署名できるアドレスのリストを作成する必要があります。

[SignerListSetトランザクション][]は、_署名者リスト_（自分のアドレスからのトランザクションを承認できるアドレスのセット）を定義します。署名者リストには、1～32のアドレスを含めることができます。このリストには、自分のアドレスを含めることはできず、重複して登録することはできません。リストの _Signer Weight_ と _Quorum_ の設定を使用することで、どのような組み合わせでどれだけの署名が必要かを制御することができます。

_([ExpandedSignerList amendment][]により更新されました。)_

### Signer Weight

リスト内の各署名者にウェイトを割り当てます。ウェイトは、リスト上の他の署名者と比較して、その署名者の相対的な権限を表します。値が高いほど、より多くの権限を持つことになります。個々のウェイト値は、2<sup>16</sup>-1を超えることはできません。

### Quorum

リストの定足数(quorum)の値は、トランザクションを承認するために必要な最小のウェイト合計です。クォーラムは0より大きく、署名者リストのウェイト値の合計以下でなければならない。つまり、与えられた署名者のウェイトでクォーラムを達成することが可能でなければなりません。

### Wallet Locator

また、リスト内の各署名者のエントリに最大256ビットの任意のデータを追加することができます。このデータはネットワークで必要とされたり使用されたりすることはありませんが、スマートコントラクトや他のアプリケーションが署名者に関する他のデータを特定したり確認したりするために使用することができます。

_([ExpandedSignerList amendment][]により追加されました。)_

### Signer WeightとQuorumの使用例

ウェイトと定足数により、アカウントを管理する責任ある参加者への相対的な信頼と権限に基づき、トランザクションごとに適切なレベルの監視を設定することができます。

共有アカウントのユースケースの場合、定足数1のリストを作成し、すべての参加者に1のウェイトを与えることができます。

非常に重要なアカウントの場合、定足数を3に設定し、重みを1とする3人の参加者を設定することができます。すべての参加者がトランザクションごとに同意して承認する必要があります。

CEOのウェイトを3、副社長3人のウェイトを各2、取締役3人のウェイトを各1に割り当てたとする。このアカウントのトランザクションを承認するには、取締役3名全員（合計ウェイト3）、副社長1名と取締役1名（合計ウェイト3）、副社長2名（合計ウェイト4）、またはCEO（合計ウェイト3）の承認が必要となります。

先の3つのユースケースでは、レギュラーキーを設定せずにマスターキーを無効にすることで、マルチシグが唯一の[トランザクションの承認](transactions.html#トランザクションの承認)の方法となるようにします。

"バックアッププラン"としてマルチシグリストを作成するシナリオがあるかもしれません。アカウント所有者は、通常、トランザクションにレギュラーキー(マルチシグではない)を使用します。もしアカウント所有者が秘密鍵を紛失した場合、通常の鍵に代わるトランザクションにマルチシグするよう友人に依頼することができます。

## マルチシグトランザクションの送信

マルチシグトランザクションを正常に送信するには、以下のすべての条件を満たす必要があります。

* トランザクションを送信するアドレス（`Account`に指定されるアドレス）は、[レジャーに`SignerList`](signerlist.html)を所有する必要があります。この方法については、[マルチシグを設定する](set-up-multi-signing.html)を参照してください。
* トランザクションに`SigningPubKey`フィールドを空の文字列として含める必要があります。
* トランザクションに、署名の配列が指定されている[`Signers`フィールド](transaction-common-fields.html#signersフィールド)を含める必要があります。
* `Signers`配列に含まれている署名は、`SignerList`で定義されている署名と一致している必要があります。
* 指定された署名で、これらの署名者に関連付けられている`weight`の合計が、`SignerList`の`quorum`以上である必要があります。
* [トランザクションコスト](transaction-cost.html)（`Fee`フィールドで指定）は、通常のトランザクションコストの（N+1）倍以上である必要があります。このNは、指定される署名の数です。
* トランザクションのすべてのフィールドは、署名収集前に定義する必要があります。フィールドの[自動入力](transaction-common-fields.html#自動入力可能なフィールド)は実行できません。
* `Signers`配列がバイナリ形式で指定される場合、この配列は署名者アドレスの数値に基づいて、低い値から順にソートされている必要があります。（JSONとして提出される場合は、[submit_multisignedメソッド][]がこの処理を自動的に実行します。）

詳細は、[マルチシグの設定](set-up-multi-signing.html)を参照してください。

## 関連項目

- **チュートリアル:**
    - [マルチシグを設定する](set-up-multi-signing.html)
    - [マルチシグトランザクションを送信する](send-a-multi-signed-transaction.html)
- **コンセプト:**
    - [暗号鍵](cryptographic-keys.html)
    - [マルチシグトランザクションの特別なトランザクションコスト](transaction-cost.html#特別なトランザクションコスト)
- **リファレンス:**
    - [SignerListSetトランザクション][]
    - [SignerListオブジェクト](signerlist.html)
    - [sign_forメソッド][]
    - [submit_multisignedメソッド][]

{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}