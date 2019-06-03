# Electronのバージョン管理

> バージョン管理ポリシーと実装の詳細をご覧ください。

バージョン2.0.0以降、Electron は [semver](#semver) に従います。 以下のコマンドは、最新の安定した Electron のビルドをインストールします。

```sh
npm install --save-dev electron
```

既存のプロジェクトを最新の安定版を使用するように更新するには、以下のようにします。

```sh
npm install --save-dev electron@latest
```

## Version 1.x

Electron バージョン *< 2.0* は、[semver](http://semver.org) 仕様に準拠していません。メジャーバージョンはエンドユーザ API の変更に対応し、マイナーバージョンは Chromium メジャーリリースに対応し、パッチバージョンは新機能およびバグ修正に対応していました。 機能を統合する開発者にとっては便利ですが、クライアント向けアプリケーションの開発者には問題が生じます。 Slack、Stride、Teams、Skype、VS Code、Atom、Desktop などのメジャーなアプリの QA テストサイクルは時間がかかることがあり、安定性においては非常に望ましい結果を出します。 これは、バグ修正を吸収しようとする一方で、新しい機能を採用することに高いリスクがあります。

1.x の方針の例を以下に示します。

![](../images/versioning-sketch-0.png)

`1.8.1` を使用して開発されたアプリケーションは、`1.8.2` の機能を取り入れるか、修正をバックポートし、新しいリリースラインをメンテナンスすることなしに、`1.8.3` のバグ修正をとることもできません。

## Version 2.0 以降

上に概説されている 1.x の方針から、いくつかの大きな変更があります。 各変更は、開発者/管理者とアプリ開発者のニーズと優先順位を満たすためのものです。

1. semver の厳格な使用
2. semver 準拠の `-beta` タグの導入
3. [conventional commit messages](https://conventionalcommits.org/) の導入
4. しっかり定義された安定ブランチ
5. `master` ブランチにはバージョンがなく、安定ブランチのみがバージョン情報を含みます。

git のブランチ動作の仕組み、npm のタグ付けの仕組み、開発者が期待するべきこと、変更をバックポートする方法について詳しく説明します。

# semver

2.0 以降から、Electron は semver に従います。

以下は、変更のタイプを対応する semver のカテゴリ (メジャー、マイナー、パッチなど) に明示的に割り当てる表です。

| メジャーバージョンの単位            | マイナーバージョンの単位              | パッチバージョンの単位         |
| ----------------------- | ------------------------- | ------------------- |
| 互換性を破る Electron API の変更 | 互換性を破らない Electron API の変更 | Electron のバグ修正      |
| Node.js のメジャーバージョン更新    | Node.js のマイナーバージョン更新      | Node.js のパッチバージョン更新 |
| Chromium のバージョン更新       |                           | Chromium パッチの修正関連   |

ほとんどの Chromium の更新は互換性を破るとみなされることに注意してください。 バックポート可能な修正は、パッチとして cherry-pick される可能性が高いです。

# 安定ブランチ

安定ブランチは、セキュリティまたは安定性に関連する cherry-pick されたコミットのみを取り入れて、master と並行して実行されるブランチです。 これらのブランチはマスターに戻されることはありません。

![](../images/versioning-sketch-1.png)

安定ブランチは、常に **major** または **minor** のバージョンラインのいずれかであり、次のテンプレート `$MAJOR-$MINOR-x` に対して `2-0-x` のように命名されます。

複数の安定化ブランチを同時に存在させることができます。また、必要に応じてセキュリティ修正を後方移植しながら、少なくとも2つのサポートを常に並行してサポートする予定です。 ![](../images/versioning-sketch-2.png)

それより古いものは GitHub ではサポートされませんが、他のグループは所有権を持って、自分自身で安定性とセキュリティの修正をバックポートできます。 これはお勧めできませんが、多くのアプリ開発者にとってライフが楽になると認識しています。

# ベータリリースとバグ修正

開発者はどのリリースが *安全* に使用できるかを知りたいものです。 一見無害な機能でさえ、複雑なアプリケーションに後退をもたらすことがあります。 同時に、あなたのバージョンから出るセキュリティパッチとバグ修正の可能性を無視しているので、固定バージョンへのロックは危険です。 私たちの目標は、`package.json` で以下のように標準的な semver 範囲を許可することです。

- `~2.0.0` を使用すると、`2.0.0` リリースに対する安定性またはセキュリティ関連の修正のみを認めます。
- `^2.0.0` を使用すると、セキュリティとバグ修正だけでなく、破壊的でない *かなり安定した* 機能も認めます。

2つ目の点に関して重要なことは、`^` を使用しているアプリはまだ妥当なレベルの安定性を期待できることです。 これを達成するために、semver の *プレリリース識別子* を特定のバージョンがまだ *安全* でも *安定* でもないことを示すことを可能にします。

どれを選択しても、破壊的な変更は Chromium が寿命である事実であるため、定期的に `package.json` 内のバージョンを更新する必要があります。

プロセスは以下の通りです。

1. すべての新しいメジャーリリースとマイナーリリースのものは、次のように、semver プレリリースタグで示されるベータ系列で始まります。 `beta.N`, 例 `2.0.0-beta.1`。最初のベータ版の後、その後のベータ版リリースは以下のすべての条件を満たす必要があります。 
    1. 変更は API に後方互換性がある (非推奨は構いません)
    2. 安定版のスケジュールを守るリスクが低くなければならない。
2. リリースがベータ版になった後に許可された変更を加える必要がある場合は、それらが適用され、例として `2.0.0-beta.2` のようにプレリリースタグが増分されます。
3. 特定のベータリリースが *一般的に安定している* と見なされている場合、バージョン情報のみを変更して、安定したビルドとして再リリースされます。 例として、`2.0.0` のようになります。 最初の安定版以降は、すべての変更は後方互換性のあるバグまたはセキュリティ修正でなければなりません。
4. リリースが安定した後に将来のバグ修正やセキュリティパッチを作成する必要がある場合は、それらを適用して、例として `2.0.1` のように、*patch* のバージョンを増やします。

具体的に言うと、以下が上記の意味です。

1. たとえそれらの変更が中程度の副作用を引き起こす可能性があるとしても、ベータサイクルの 3 週間前の段階で非破壊的な API の変更を承認することは問題ありません。
2. ベータサイクルのほとんどの時点で、既存のコードパスを変更しない、機能フラグの変更を認めることは問題ありません。 ユーザーは自分のアプリでこれらのフラグを明示的に有効にできます。
3. Admitting features of any sort after Week 3 in the beta cycle is 

メジャーとマイナーのバージョン上げのそれぞれにおいて、以下のようなものが見えるはずです。

```text
2.0.0-beta.1
2.0.0-beta.2
2.0.0-beta.3
2.0.0
2.0.1
2.0.2
```

以下は絵に描いたライフサイクルの例です。

- 最新の一連の機能を含む新しいリリースブランチが作成されます。`2.0-beta.1` として公開されます。 ![](../images/versioning-sketch-3.png)
- リリースブランチにバックポートできるバグ修正が master に入ってきました。 パッチが適用され、新しいベータ版が `2.0-beta.2` として公開されます。 ![](../images/versioning-sketch-4.png)
- このベータ版は *一般的に安定している* と見なされています。そして `2.0.0` の下に非ベータ版として再度公開されます。 ![](../images/versioning-sketch-5.png)
- その後、ゼロデイ攻撃が明らかになり、修正が master に適用されます。 この修正を `2-0-x` 系列にバックポートし、`2.0.1` をリリースします。 ![](../images/versioning-sketch-6.png)

以下は、さまざまな semver 範囲の新しいリリースの拾い方のいくつかの例です。

![](../images/versioning-sketch-7.png)

# 失なわれた機能: アルファ

私たちの戦略にはいくつかのトレードオフがありますが、今のところそれは適切だと感じています。 最も重要なことは、master の新機能が安定したリリースラインに到達するまでにはしばらく時間がかかることです。 すぐに新しい機能を試したい場合は、自分で Electron をビルドする必要があります。

今後の検討事項として、以下のうちの一方または両方を紹介する可能性があります。

- ベータ版に対する安定性の制約が緩いアルファリリース。 例えば、安定チャネルが *alpha* の間に新しい特徴を認めることが許容される。

# 機能フラグ

機能フラグは Chromium で一般的な方法であり、Web 開発エコシステムではよく確立されています。 Electron のコンテキストでは、機能フラグまたは **ソフトブランチ** には次のプロパティが必要です。

- 実行時またはビルド時に有効/無効になるもの。リクエストスコープ付き機能フラグの概念はサポートしていない
- 新旧のコードパスを完全に断片化するもの。 新しい機能をサポートするために古いコードをリファクタリングすると機能フラグ規約に *違反する*
- 機能のリリース後、機能フラグは最終的に削除される

# セマンティックなコミット

私達は更新およびリリースプロセスのすべてのレベルで明快さを増すよう努めます。 `2.0.0` 以降、プルリクエストは [従来のコミット](https://conventionalcommits.org/) の仕様に準拠する必要があります。これは以下のようにまとめることができます。

- **major** バージョン上げをするコミットは `BREAKING CHANGE:` の本文で始まる必要があります。
- **minor** バージョン上げをするコミットは `feat:` で始まる必要があります。
- **patch** バージョン上げをするコミットは `fix:` で始まる必要があります。

- squash されたメッセージも上記のメッセージフォーマットを遵守するという条件で、我々はコミットの squash を許します。

- プルリクエストのタイトルが意味のある包括的なセマンティックメッセージを含むのであれば、プルリクエストにおけるいくつかのコミットがセマンティックプレフィックスを含まないことは許容できます。

# バージョン付けされた `master`

- `master` ブランチは、常に `package.json` に次のメジャーバージョンの `X.0.0-nightly.DATE` を含みます。
- リリースブランチが master にマージし戻されることはありません。
- リリースブランチは `package.json` 内に正しいバージョンを含んで *います*
- リリースブランチがメジャーのためにカットされるとすぐに、master は次のメジャーにバージョン上げされる必要があります。 すなわち、`master` は常に理論上次のリリースブランチとしてバージョン管理されます