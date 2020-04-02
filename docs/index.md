# MkDocs の見出しを Font Awesome で装飾

Font Awesome のアイコンが見出しの頭にくっつくようにして、なんとなくかわいい感じにします。

このページはそのライブデモです。上の見出しには :fa-book:、下の見出しには :fa-arrow-circle-right: がついています。


## 考え方

実現方法には、

* 愚直に Markdown 中の見出し行（`#` とか `##` とか）に Font Awesome の記述を埋め込む案
* CSS の `::before` 疑似要素で流し込む案

がありますが、装飾目的のモノを本文に混ぜこむことはマークアップの思想に反する気がするので、後者を採用します。`fontawesome_markdown` とも共存可能です。

わりと強引な手なので、本来いちばんお行儀がよいのは、おそらく専用の拡張機能を開発することな気もしていますが、使いたい範囲では使えているのでひとまずよしとします。

!!! note "オーバライドの必要性"
    `Material` テーマでは、見出しの `::before` 疑似要素が事前定義されているため、一部の設定は `!important` を指定してオーバライドする必要があります。

!!! warning "オーバライドの危険性"
    オーバライドする必要があるとは言ったものの、当然ながら副作用を誘発している可能性はあり、かつ調べきれていないので、ほどほどに自己責任で使ってください。
    
    レスポンシブでの挙動やスマートフォンでの閲覧、印刷モード、IE など、簡単に確認している範囲では問題なさそうです。


## 手順

`mkdocs.yml` の `extra_css` で Font Awesome の CSS ファイルと自前の CSS ファイルを指定して、

```yaml
extra_css:
    - "https://stackpath.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css"
    - "css/custom.css"
```

自前の CSS ファイルの中で、`::before` 疑似要素を定義します。

例えば、`h2` の見出し（`##`）の頭に :fa-arrow-circle-right: をくっつけたい場合は、[一覧で調べる](https://fontawesome.com/v4.7.0/icon/arrow-circle-right) と Unicode が `f0a9` なので、

```css
/* Materials テーマの場合 */
.md-typeset h2::before {
    display: inline-block !important;
    font-family: "FontAwesome";
    content: "\f0a9" !important;
    margin-right: .3em;
}
```

```css
/* デフォルトのテーマの場合 */
h2::before {
    font-family: "FontAwesome";
    content: "\f0a9";
    margin-right: .3em;
}
```

な具合で記述します。もちろん `margin-right` は必須ではないですが、多少はあったようが目に優しい気がします。同様に、色やサイズも適宜変更できます。

複数の見出しレベルを装飾したい場合は、副作用に気を付けつつ共通の指定はまとめるのもアリです。次の例では、`h1` に :fa-book:、`h2` に :fa-arrow-circle-right: を追加しています。
```css
.md-typeset > ::before {
    font-family: "FontAwesome";
    margin-right: .3em;
    display: inline-block !important;
}

.md-typeset h1::before {
    content: "\f02d " !important;
}

.md-typeset h2::before {
    content: "\f0a9" !important;
}
```

!!! warning "まとめた場合の副作用"
    もちろん、副作用の範囲は広がるので、意図しない部分の表現が壊れる可能性があります。
    
    上記では、`>` で `.md-typeset` クラスの直接の子要素の `::before` 疑似要素にのみ作用させているので、逆に何らかのブロック要素に見出しを含めた場合は期待する表示にならない可能性もあります。


## アイコンを探す

CSS で `4.7.0` を指定したので、[公式ページの `4.7.0` の一覧ページ](https://fontawesome.com/v4.7.0/icons/) が対応します。それぞれのアイコンのリンク先で Unicode が確認できます。


## Font Awesome 5 を使いたい場合

上記は `4.x` 系の例ですが、最新の `5.x` 系にしたい場合は、

* CSS を `https://use.fontawesome.com/releases/v5.13.0/css/all.css` などにする
* フォント名を `Font Awesome 5 Free` にする

と動作します。この場合の [アイコンの一覧は別のページ](https://fontawesome.com/icons) です。

`4.x` 系は古いですが、本来 `5.x` 系はユーザ登録が必要なのと、[MkDocs の公式サイト](https://www.mkdocs.org/) では `4.7.0` なのと、を理由に今回も `4.7.0` で紹介しています。


## 見出しサンプル（レベル 2）

吾輩は猫である。名前はまだ無い。

* どこで生れたか
    * とんと見当がつかぬ。
    * 何でも薄暗いじめじめした所でニャーニャー泣いていた事だけは記憶している。
* 吾輩はここで始めて人間というものを見た。


### 見出しサンプル（レベル 3）

しかもあとで聞くとそれは書生という人間中で一番獰悪な種族であったそうだ。


### 見出しサンプル（レベル 3）

この書生というのは時々我々を捕えて煮て食うという話である。

    しかしその当時は何という考もなかったから別段恐しいとも思わなかった。
    ただ彼の掌に載せられてスーと持ち上げられた時何だかフワフワした感じがあったばかりである。


#### 見出しサンプル（レベル 4）

掌の上で少し落ちついて書生の顔を見たのがいわゆる人間というものの見始であろう。この時妙なものだと思った感じが今でも残っている。


#### 見出しサンプル（レベル 4）

第一毛をもって装飾されべきはずの顔がつるつるしてまるで薬缶だ。その後猫にもだいぶ逢ったがこんな片輪には一度も出会わした事がない。のみならず顔の真中があまりに突起している。

