---
title: "Reactで使えるMaterial DesignのSpinnerコンポーネントを作った"
slug: "react-md-spinner"
date: "2016-07-03"
categories: ["JavaScript"]
image: ""
draft: true
---

Googleの提唱する[Material Design](https://material.google.com/)チックなSpinnerを、Reactコンポーネント化して公開しました。

リポジトリは以下です。

> tsuyoshiwada/react-md-spinner  
> https://github.com/tsuyoshiwada/react-md-spinner



## デモ

![デモンストレーション]({{% image "demo.png" %}})

> React MD Spinner - DEMO  
> https://tsuyoshiwada.github.io/react-md-spinner/

実際の動きは以下の様にカラフルな円形がくるくる回る感じになっています。

![gifイメージ]({{% image "demo.gif" %}})

手持ちのiPhone6sで確認している限り、スマートフォンでも必要充分スムーズに動作しています。



## 使い方

`react-md-spinner`の特長として、[material-ui](http://www.material-ui.com/)の様にインラインスタイルのみで構成されるため、インストールしたらCSSどうしようみたいに悩まずにすぐ使い始められる点があります。

### インストール

npmに公開したので、他のモジュールと同様に`npm install`します。

```bash
$ npm install react-md-spinner
```


### 基本的な使用例

全てのプロパティがOptionalなので特に設定なく使えます。

```javascript
import React, { Component } from "react";
import MDSpinner from "react-md-spinner";

export default class SpinnerExample extends Component {
  render() {
    return (
      <div>
        <MDSpinner />
      </div>
    );
  }
}
```

`react-md-spinner`は単純にくるくるしているだけなので、表示/非表示みたいな操作はアプリに応じて使用者が実装する必要があります。


### プロパティ

指定可能なするプロパティは以下の通りです。

| プロパティ    | 型             | デフォルト値                                                                                                   | 概要                                                                            |
|:--------------|:---------------|:---------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------|
| `className`   | string         | undefined                                                                                                      | コンポーネントに対して`className`を設定します。                                 |
| `style`       | object         | undefined                                                                                                      | MDSpinner内のルート要素に対してスタイルを設定します。                           |
| `size`        | number, string | 28                                                                                                             | スピナーのサイズを設定します。                                                  |
| `duration`    | number         | 1333                                                                                                           | アニメーションの速度を設定します。                                              |
| `color1`      | string         | ![color1]({{% image "color1.png" %}}) rgb(66, 165, 245) | スピナーの色をCSSに有効な文字列で設定します。                                   |
| `color2`      | string         | ![color2]({{% image "color2.png" %}}) rgb(239, 83, 80)  | 同上                                                                            |
| `color3`      | string         | ![color3]({{% image "color3.png" %}}) rgb(253, 216, 53) | 同上                                                                            |
| `color4`      | string         | ![color4]({{% image "color4.png" %}}) rgb(76, 175, 80)  | 同上                                                                            |
| `singleColor` | string         | undefined                                                                                                      | 基本は`color1`~`color4`と同様ですが、単色指定を行うと他の色設定は無視されます。 |

サイズや色味を変更可能な為、使う場面やアプリに応じたスタイルを適用できる様に柔軟性を持たせてみました(つもり)。



## 技術的な点

今進めている趣味プロジェクトがMaterial Designを参考にしたデザインで進めているのですが、巨大なロックインを避ける為にUI周りのフレームワークは極力使わず、小さい単位でのコンポーネントを採用する方針で進めています。しかし、単体で使用できるSpinnerにあんまりいいのが無いなぁという事で、設定いらずですぐに使える、という点を重視して作成しました。

使用者にwebpackのstyle-loaderなんかを強制したくなくて、スタイル周りはシンプルにInline Stylesを採用しています。


### 問題点

そこで問題となったのが、くるくる+色味の変化を行うアニメーション部分でした。一般的に挙げられる解決策として以下の様な方法があるかと思います。

1. CSSを別ファイル用意してその中で`@keyframes`を定義して、スタイルを割り当てる(ユーザに直接読み込んでもらうか、CSSModulesなど)
2. JS側で`requestAnimationFrame`などを駆使してアニメーションさせる
3. JS側で動的に`@keyframes`を作成し`head`に`style`要素を追加、各要素へスタイルを割り当てる

1は使用者の環境へ設定が必要となってしまうのでコンセプトとずれるため却下。2はJS的なアプローチですが、複数のSpinnerを表示した際に、同一のアニメーションなのにそれぞれの要素に対して各フレームスタイルを書き換えていくのは非効率です(あまり無いケースとは思いますが...)。

消去法で3の方法を使い実装しました。


### JSで動的に@keyframesを追加する

このアプローチ方法については、ググれば幾つかの方法が出てきます。`react-md-spinner`では、この処理部分のみ別のパッケージとして切り出して公開しました。(コンポーネントのテストとは分離したかったので)

> tsuyoshiwada/css-keyframer  
> https://github.com/tsuyoshiwada/css-keyframer

指定したスタイルに必要なベンダープレフィックスを自動的に付与したり、`@keyframes`の登録/登録解除といった必要最低限のAPIを実装したものです。


## まとめ

見た目は限りなくシンプルなので、Material DesignのSpinnerとして公開しましたが、色味を変更すれば幅広いデザインに適用出来そうなので、Spinner探してたのよね〜という方に是非使ってみて欲しいなと思います。  
バグや機能改善は[Issues](https://github.com/tsuyoshiwada/react-md-spinner/issues)や[Twitter](https://twitter.com/wadackel)までいただけると嬉しいです。

> tsuyoshiwada/react-md-spinner  
> https://github.com/tsuyoshiwada/react-md-spinner