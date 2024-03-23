---
html: algorithmic-trading.html
parent: defi-uc.html
seo:
    description: XRP Ledgerの分散型取引所は、ユーザがトレードを行う際にオンデマンドで追跡される無制限の通貨ペアで構成されています。
labels:
  - トランザクション
---
# アルゴリズムトレード

XRP Ledgerの分散型取引所(DEX)には、「アルゴリズムトレード」によって収益を得る機会があります。アルゴリズムトレードでは通常、定量的な要因に基づいて多くのトレードを行い、安定した小さな利益を得ます。これは、市場のファンダメンタルズに基づいて少数の長期投資を行い、時間をかけて大きなリターンを得るのを待つ伝統的な手動トレードとは異なります。暗号通貨は一般的にボラティリティが高いため、伝統的な「バイ・アンド・ホールド」投資には適していませんので、ブロックチェーンは多くの場合、手動トレードよりもアルゴリズムトレードに適しています。

- 分散型取引所のデータはパブリックで自由に利用できます。
- 取引は数秒で決済されるため、専門的な機材なしで高頻度トレードが可能です。
- XRP Ledgerのトランザクションコストは低額です。

## トレード戦略

アルゴリズムトレードは、さまざまな戦略によって利益を上げることができます。アルゴリズムトレードの難しさ（あるいは楽しさ）の一つは、独自のアプローチを設計し、実行することです。アルゴリズムトレードのアプローチを大まかに分類すると、次のようになります。

- **裁定取引(アービトラージ):** 価格差を利用した資産の購入と即時売却。これは、価格が一致していない3つ以上の資産のセットを見つけたり、XRP Ledgerを使って価格が異なる取引所(CEX)間で資産を移動させたりすることを含みます。
- **定量取引:** 過去の値動き、外部データ、またはその両方に基づいて将来の値動きを予測し、それを利用すること。例えば、[ローソク足パターン](https://blog.quantinsti.com/candlestick-patterns-meaning/)、資産の値動きと他の資産との相関関係、ソーシャルメディアのセンチメント分析などがあります。
- **フロントランニング:** 保留中のトレード、特に大きなトレードを利用し、そのトレードが買っている資産を買ってすぐに転売すること。フロントランニングは、流動性を追加することなく他のトレーダーから利益を奪ったり、他の方法では発生しないトレードを可能にするため、しばしば嫌われます。XRP Ledgerの取引の正規順序は、フロントランニングを困難にしていますが、不可能ではありません。

### 裁定取引(アービトラージ)の例

裁定取引を行うには、XRP Ledgerの内部と関連する部分の両方で、多くの方法があります。以下の例は潜在的な戦略を説明するためのものですが、他の方法も可能です。

**循環支払い**を利用して、マルチアセットトレードを完了し利益を得ることができます。XRP Ledgerは、XRPが真ん中のアセットである3つのアセットセットセットと同様に、アセットペア間の重複した取引を自動的に接続します。しかし、XRP Ledgerのプロトコルは、他のより長い、あるいはより複雑な経路のトレードを自動的に見つけて競うことはしません。(可能な限り最善の経路を見つけることは、計算集約型な問題のカテゴリとして知られています。)したがって、XRP Ledgerが独自の経路を見つける場合、XRP Ledgerのプロトコルは自動的に他の、より長い、あるいはより複雑な経路の取引を見つけ、競争させることはありません。したがって、自分で経路探索(PathFinding)を行えば、このような有益な裁定取引の機会を見つけることが可能です。その場合は、[Paymentトランザクション](../../references/protocol/transactions/types/payment.md)でそれらの[経路(Paths)](../../concepts/tokens/fungible-tokens/paths.md)を明示的に指定できます。1つのFOOを使って2つのBARを買い、その2つのBARを使って3つのTSTを買い、最後に3つのTSTを使って1.1 FOOを買えば、0.1 FOOから取引に関わるトークンの[送金手数料](../../concepts/tokens/transfer-fees.md)などのコストを差し引いた利益を得ることができます。

資産の価格が異なる複数の取引所(CEX)に口座を持っている場合、**取引所間の裁定取引**を行うことができます。例えば、ACME取引所でXRPを1XRPあたり0.45ドルで購入し、そのXRPをWayGate取引所に移動して1XRPあたり0.50ドルで売却した場合、XRPあたり0.05ドルの利益を得ることができます。より複雑な例として、ACME取引所でBTC:ETHの価格が変動し、BTCに対してETHが安くなった場合、ある取引所でETH→XRPを売却し、そのXRPをACME取引所に移動し、XRP→BTC→ETHを取引して利益を得ることで、この価格変動を利用できる可能性があります。XRP Ledgerの取引は数秒で決済されますが、イーサリアムの取引は数分、ビットコインの取引は数時間かかることがあるため、XRPをブリッジ通貨として使用することで、ACME取引所でETH→BTC→BTC→ETHと取引するよりも早くこの機会を利用できる可能性があります。(これはもちろん、XRPへの交換が利益以上のコストにならないだけの十分な流動性と狭いスプレッドがある場合にのみ機能します)


## 背景

アルゴリズムトレードの一般的な知識については、以下のリソースをご覧ください。

- [Investopedia: Basics of Algorithmic Trading: Concepts and Examples](https://www.investopedia.com/articles/active-trading/101014/basics-algorithmic-trading-concepts-and-examples.asp)
- [_Flash Boys: A Wall Street Revolt_ by Michael Lewis](https://wwnorton.com/books/Flash-Boys/)
- [Investopedia: How Arbitraging Works in Investing, With Examples](https://www.investopedia.com/terms/a/arbitrage.asp)

以下のページでは、XRP Ledgerの分散型取引所の仕組みについて説明しています。

- [トークン](../../concepts/tokens/index.md)
- [分散型取引所](../../concepts/tokens/decentralized-exchange/index.md)
- [オファー](../../concepts/tokens/decentralized-exchange/offers.md)


## テストとよくある間違い

どのような取引でもそうですが、アルゴリズムトレー ドは確実に儲かる方法ではありません。手作業によるトレードと比べると、アルゴリズムトレードはエラーの余地が非常に少なくなります。小さなミスを犯しても、そのミスを大量のトレードで倍増させようとすれば、問題を修正する前に損失があっという間に膨らんでしまいます。したがって、自分のトレード戦略が実際に利益を上げるかどうかを確認するために、さまざまなテストを行うのが賢明です。戦略やその実際の実装（よく _ボット_ と呼ばれます）をテストするために、次のようなことを行うことができます。

- 現在のレジャーの状態または過去のトレードに基づいて、潜在的な利益を手動で計算します。
- 過去のデータを記録してボットに送り、ボットがどのようなアクションを取ったかを記録し、実際の過去の値動きと結果を比較します。
- 将来の様々なシナリオをモデル化し、その結果を予測します。

このような計算やボットの作成においては、よく次のような間違いが存在します。

- **丸め誤差**: 計算が適切でなかったり、ブロックチェーンが使用する精度と一致しなかったりすると、トレード結果を不正確に予測して損失を出したり、トレードが全く実行されなかったりする可能性があります。XRP Ledgerは、トークンとXRPの量に異なる精度を使用しているため、一方を他方に交換する際、予期せぬ場所で四捨五入される可能性があります。プロトコルで使用される精度の詳細については、[通貨フォーマット](../../references/protocol/data-types/currency-formats.md)をご覧ください。
    - トークンの発行者は、トークンに関わる取引レートの精度をより詳細に制限できることに注意してください。詳しくは[Tick Size](../../concepts/tokens/decentralized-exchange/ticksize.md)をご覧ください。
    - 通常、四捨五入の違いや、計算時と約定時の値動きの違いを考慮し、金額を調整する必要があります。この金額は「スリッページ」と呼ばれ、適切な金額を設定することが重要です。スリッページが低すぎると、トレードがまったく約定しない可能性があります。一方、スリッページが高すぎると、フロントランニングの影響を受けやすくなり、スリッページが高ければ高いほど、値動きによって利益が削られる可能性が高くなります。
- **余分なコストと遅延を考慮しないこと**: 例えば、2つのステーブルコインの裏付けが米ドルであるにもかかわらず、ある発行者が0.5%の送金手数料を請求し、別の発行者が0.25%の[送金手数料](../../concepts/tokens/transfer-fees.md)を請求した場合、そのステーブルコインの取引価格には約0.25%の差が生じます。トランザクションを送信するためのコストは、通常は少額ですが、その他の潜在的な遅延の影響も忘れないでください。例えば、オフレジャーの取引所が現時点で有利な価格を示していたとしても、その取引所の入金処理に数時間から数日かかる場合、その取引所で事前に流動性を持っていない限り、その価格を利用することはできません。
- **稀な事象を考慮していないこと**: 前例のない出来事（「ブラック・スワン」）はさておき、個々の異常値によって計算結果がゆがむことがあります。一例として(これは実話ですが)、あるトレーダーが、ある戦略の潜在的な利益を特定の時間帯で計算したところ、利益の80％以上が、他のユーザが誤って価格にゼロを追加してしまった1つの「入力ミス」の取引によるものであったと報じました。同じ戦略を、これらの異常値の取引を含まない時間範囲に対して計算すると、利益ははるかに少なくなりました。
- **トランザクションのフラグを確認しないこ**と: XRP Ledgerのトランザクションのフラグは、そのトランザクションの処理方法や、プロトコルがそれを「成功」とマークするタイミングに大きな影響を与える可能性があります。例えば、"Offer"トランザクションのフラグは、全額がすぐに得られる場合にのみトレードされる"Fill or Kill"注文にすることができます。"Payment"トランザクションのフラグは、意図した宛先に全額を届けることができなくても成功する[partial payments](../../concepts/payment-types/partial-payments.md)にすることができます。トランザクションの`Flags`フィールドを解析するためにビット演算をする必要がありますが、それをスキップしてしまうと、予想と結果が全く異なったものとなってしまう可能性があります。

## 税金とライセンス

ブロックチェーン上でトレードを行うための法的要件は、法域によって異なります。多くの場合、ライセンスやその他の法的な障壁はありませんが、特に利益または損失がいくつかの基準値を超える場合、税務上の利益を報告する必要がある場合があります。米国では通常、トレードで得た利益(または損失)をキャピタルゲインとして報告します。つまり、購入した資産の取得時の原価を計算する必要があります。個々の状況に応じて、トレード活動を追跡したり、適切な納税申告書を作成するのに役立つ様々なツールがあります。トレードする資産やトレード戦略によって、詳細は異なります。アルゴリズムトレードを始める前に、必ず税務の専門家に相談するか、よく調べてください。


## 技術的な詳細

### トレードの発注

XRP Ledgerの分散型取引所で_代替可能_トークンとXRPを売買するには、通常[OfferCreateトランザクション](../../references/protocol/transactions/types/offercreate.md)を送信します。この方法でトレードを行うためのコードと技術的ステップの詳細なウォークスルーについては、[分散型取引所でのトレード](../../tutorials/how-tos/use-tokens/trade-in-the-decentralized-exchange.md)をご覧ください。[Paymentトランザクション](../../references/protocol/transactions/types/payment.md)を使用して通貨を両替することも可能です。[クロスカレンしー支払い](../../concepts/payment-types/cross-currency-payments.md)を他のユーザに送ったり、長い[パス](../../concepts/tokens/fungible-tokens/paths.md)を使って裁定取引の機会を1つの操作にまとめることで、自分自身に送り返すこともできます。

NFTをトレードするためのコードと技術的な手順については、[JavaScriptを使用したNFTokenの送信](../../tutorials/javascript/nfts/transfer-nfts.md)を参照してください。

### トレードデータの確認

XRP Ledgerのトレード活動に関する情報源は数多くあります。トレード戦略やユースケースによっては、[公開サーバ](../../tutorials/public-servers.md)を通してXRP Ledgerに接続することができるかもしれませんが、多くの場合、自分自身のサーバを稼働させることで利益を得ることができます。P2Pモードでコアサーバをセットアップする方法については、[`rippled`のインストール](../../infrastructure/installation/index.md)をご覧ください。

他のトレード活動を追跡するアプローチの場合、トレード額を正確に知るためにトランザクションの詳細なメタデータを確認する必要があるかもしれません。オファーは部分的に約定することがあり、複数の一致するオファーを約定することがあります。トランザクションメタデータの解釈方法の詳細については、[トランザクションの結果の確認](../../concepts/transactions/finality-of-results/look-up-transaction-results.md)をご覧ください。

利益確定のチャンスに反応する時間をできるだけ多く確保するために、[オープンレジャー](../../concepts/ledgers/open-closed-validated-ledgers.md)から保留中のデータを見たり、提案されているトランザクションをモニタリングしたりすることもできます。WebSocketに接続している場合、`transactions_proposed`ストリームで[subscribeメソッド](../../references/http-websocket-apis/public-api-methods/subscription-methods/subscribe.md)を使用すると、コンセンサスによって検証される前のトランザクションを見ることができます。また、`accounts_proposed`パラメータを使用してsubscribeすることで、特定のアカウント（例えば、トレードに興味のあるトークンの発行者）に影響するトランザクションのサブセットに限定することもできます。

### 今後の展開

Ripple社はXRP Ledger プロトコルを拡張し、既存の中央指値注文ベース(CLOB)の分散型取引所と連携するネイティブな自動マーケットメーカー(AMM)の機能を追加することを提案しました。この提案が受け入れられ、[amendments](../../concepts/networks-and-servers/amendments.md)として有効になれば、AMMはXRP Ledger上のトレードにおいて重要な要素となるでしょう。詳しくは以下のリンクをご覧ください。

- [XLS-30d: Automated Market Maker 規格草案](https://github.com/XRPLF/XRPL-Standards/discussions/78)
- [AMMドキュメント](../../concepts/tokens/decentralized-exchange/automated-market-makers.md)

## さらに詳しく

次の記事では、これらの戦略が他のブロックチェーンでどのように機能するかについて、より具体的な例や興味深い情報を提供しています。これらの情報は、始めるために必要なものではありませんが、新たな視点を得るのに役立つかもしれません。

- [Ethereum is a Dark Forest](https://www.paradigm.xyz/2020/08/ethereum-is-a-dark-forest)
- [Flash Boys 2.0: Frontrunning, Transaction Reordering, and Consensus Instability in Decentralized Exchanges (PDF)](https://arxiv.org/pdf/1904.05234.pdf)
- [Slippage in AMM Markets](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4133897)
- [Frontrunner Jones and the Raiders of the Dark Forest: An Empirical Study of Frontrunning on the Ethereum Blockchain](https://www.usenix.org/conference/usenixsecurity21/presentation/torres)
- [SoK: Transparent Dishonesty: front-running attacks on Blockchain (PDF)](https://arxiv.org/pdf/1902.05164)