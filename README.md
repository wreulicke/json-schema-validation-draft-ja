JSON Schemaの規格の邦訳
====================================

JSON Schemaの普及のためにこのリポジトリは作成されました。[WIP]
この文書は[このリンク](https://tools.ietf.org/html/draft-fge-json-schema-validation-00)の邦訳です。

# 概要

JSON Schema (application/schema+json)には様々な狙いがあり、その一つがデータのバリデーションです。
バリデーション処理は対話的に行われるかもしれませんし、対話的には行われないかもしれません。
例えば、アプリケーションでユーザの入力チェックをしながら動的なコンテンツ生成を可能とするユーザインターフェースを作ったり
様々なソースから得たデータのバリデーションをしたりするためにJSON Schemaを使うケースが考えられます。
この仕様書はバリデーション処理に用いられるのスキーマのキーワードについて記述します。

# Status of This Memo
この部分に関しては翻訳いたしません。
[原文](http://json-schema.org/latest/json-schema-validation.html)を御覧ください。

# Copyright Notice
この部分に関しては翻訳いたしません。
[原文](http://json-schema.org/latest/json-schema-validation.html)を御覧ください。
また著作権については原文書に帰属します。

# 目次

* 導入 - Introduction
* 表記と用語 - Conventions and Terminology
* 相互運用性に関する検討 - Interoperability considerations
    * 文字列の場合のバリデーション - Validation of string instances
    * 数値の場合のバリデーション - Validation of numeric instances
    * 正規表現 - Regular expressions
* 一般的なバリーションに関する検討 - General validation considerations 
    * キーワードとデフォルト型の例 - Keywords and instance primitive types
    * 相互依存キーワード - Inter-dependent keywords
    * キーワードが存在しない場合のデフォルト値 - Default values for missing keywords
    * コンテナの場合のバリデーション - Validation of container instances
* 型ごとのバリデーションキーワード(?) - Validation keywords sorted by instance types
    * 数値の場合 - Validation keywords for numeric instances (number and integer)
        * multipleOf キーワード
        * maximum and exclusiveMaximum
        * minimum and exclusiveMinimum
    * 文字列の場合 - Validation keywords for strings
        * maxLength
        * minLength
        * pattern
    * 配列の場合 - Validation keywords for arrays
        * additionalItems and items
        * maxItems
        * minItems
        * uniqueItems
    * objectの場合 - Validation keywords for objects
        * maxProperties
        * minProperties
        * required
        * additionalProperties, properties and patternProperties
        * dependencies
    * その他型におけるキーワード - Validation keywords for any instance type
        * enum
        * type
        * allOf
        * anyOf
        * oneOf
        * not
        * definitions
* メタデータに関するキーワード - Metadata keywords
    * タイトルと説明 - "title" and "description"
        * 正常値 - Valid values
        * 狙い - Purpose
    * デフォルト - "default"
        * 正常値 - Valid values
        * 狙い - Purpose
* 形式を持つセマンティックバリデーション(?) - Semantic validation with "format"
    * 前置き - Foreword
    * 実装要件 - Implementation requirements
    * 定義されている属性 - Defined attributes
        * date-time
        * email
        * hostname
        * ipv4
        * ipv6
        * uri
* 内部構造特定のための参照アルゴリズム - Reference algorithms for calculating children schemas
    * 前置き - Foreword
    * 配列要素 - Array elements
        * 定義されている特徴 - Defining characteristic
        * 影響されるキーワードとデフォルト値について - Implied keywords and default values
        * 型定義方法 - Calculation
    * Objectの要素 - Object members
        * 定義されている特徴 - Defining characteristic
        * 影響されるキーワード - Implied keywords
        * 型定義方法 -Calculation
* セキュリティに関する検討 - Security considerations

（邦訳する予定のないコンテンツについては省略しています。）

# 導入 - Introduction

JSON Schemaは与えられたJSONデータに対して特定の基準を満たすことを要求するために利用することが可能です。
この仕様に示されるキーワード郡によって、それら基準を具体的に示すことが可能です。
また動的なデータの生成を助けるためにキーワード郡が定義されています。
それらについてもこの仕様に記載します。

この仕様はJSON Schema Coreの仕様によって定義されている用語が使用されます。
Coreの仕様書についても確認できるような状況で読まれることをお勧めします。

# 表記と用語 - Conventions and Terminology

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY",
and "OPTIONAL" in this document are to be interpreted as described in RFC 2119 [RFC2119].
(和訳しません。)

この仕様においてコンテナ要素（"container instance"）とは配列やオブジェクトを示す用語です。
また、子要素("children instance")とは配列の要素、もしくはオブジェクトのメンバを表す用語です。

この仕様書ではプロパティセット（"property set"）とはオブジェクトのメンバ名のセットを表す単語です。
例えば、{ "a": 1, "b": 2 } というJSONオブジェクトのプロパティセットは [ "a", "b" ] です。

配列の要素がもしどの要素の２つ組も等しくない場合、コア仕様にあるように、ユニーク（一意）である、と表現されます。

# 相互運用性に関する検討 - Interoperability considerations
## 文字列の場合のバリデーション - Validation of string instances

JSONにおける文字列でヌル文字(\x00)は妥当な値であることに留意すべきです。
バリデートする値は、プログラミング言語にこのようなデータを取り扱う能力に関わらずこの文字を含んでいるかもしれません。

## 数値の場合のバリデーション - Validation of numeric instances

JSONの仕様では数値の精度や値域に関する定義はされていません。
JSON SchemaもJSONと同じようにそれらについては定義しません。
これはプログラミング言語の能力の有無に関わらず、
JSON Schemaによって与えられた数値は任意の大きさを持つことができ、任意のサイズの小数部を持つことを意味しています。

## 正規表現 - Regular expressions

"pattern"および"patternProperties"のバリデーションキーワードでは制約を表現するために正規表現を使います。
これらの正規表現はECMA262の正規表現の記述方式に従って、正しくなければなりません。

さらに、正規表現の実装によって大きな差異をもたらされるため、
スキーマの著者は次の正規表現トークンの利用のみに限られるべきです。

* 個別のユニコード文字：JSONの定義に基づく。 - individual Unicode characters, as defined by the JSON specification [RFC4627];
* 簡易文字クラス - simple character classes ([abc]), range character classes ([a-z]);
* 相補文字クラス - complemented character classes ([^abc], [^a-z]);
* 簡易数量子 - simple quantifiers: "+" (one or more), "*" (zero or more), "?" (zero or one), and their lazy versions ("+?", "*?", "??");
* 範囲数量子 - range quantifiers: "{x}" (exactly x occurrences), "{x,y}" (at least x, at most y, occurrences), {x,} (x occurrences or more), and their lazy versions;
* 文頭・文末 - the beginning-of-input ("^") and end-of-input ("$") anchors;
* グループ化と選択子 - simple grouping ("(...)") and alternation ("|").

最後に、正規表現が文頭文末どちらにも固定されていると考慮される実装であるべきではありません。
これは例えば、"es"の場合は"expr**es**sion"にマッチすることを意味します。