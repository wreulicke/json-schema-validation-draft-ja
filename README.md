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
    * キーワードとプリミティブ型の例 - Keywords and instance primitive types
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
        * 型同定方法 -Calculation
* Security considerations
* Security considerations
* IANA Considerations
* References
* Appendix A.  ChangeLog

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

# 一般的なバリーションに関する検討 - General validation considerations 

## キーワードとプリミティブ型の例 - Keywords and instance primitive types

いくつかのバリデーションキーワードは一つ以上のプリミティブ型に適用されます。
与えられたキーワードによってバリデーションできない場合、
このキーワードと値のバリデーションは正常であるべきです。

（？）型に応じてグループ分けされたキーワードのこの仕様は、異なるセクションでバリデーションします。
全ての型に関するバリデーションを行う、いくつかのキーワードに留意してください。

## 相互依存キーワード - Inter-dependent keywords

値をバリデーションするために他のキーワードの存在（もしくは不在）が影響することがあります。
この場合、これらの影響を与える全てのキーワードは同じセクションとして扱われます。

## キーワードが存在しない場合のデフォルト値 - Default values for missing keywords

いくつかのキーワードは、なかったとしても、デフォルト値として実装されているとみなされることがあります。
この場合、デフォルト値は別記されます。

## コンテナの場合のバリデーション - Validation of container instances

コンテナ要素をバリデートするキーワードはそれ自身のみをバリデートし、子要素についてはいません。
（？）いくつかのキーワードは, バリデーションしますが、それらは子要素が正しいかどうかを計算するために必要な情報を含んでいます。
子要素に関するSchema定義は分離して説明されます。

配列の要素は１つのスキーマに対してのみバリデートする必要があることに対し
オブジェクトのメンバは一つ以上のスキーマに対してバリデートする必要があることに注意してください。

# 型ごとのバリデーションキーワード(?) - Validation keywords sorted by instance types

## 数値の場合 - Validation keywords for numeric instances (number and integer)

### multipleOf キーワード

#### 正常値
このキーワードの値は数値型でなければなりません。
必ず０よりも大きい値でなければなりません。

#### 正常値である場合の条件

数値が"multipleOf"で割った場合に整数値であるのなら正常値です。

### maximum and exclusiveMaximum

#### 正常値

"maximum"は数値でなければいけません。
"exclusiveMaximum"は真偽値でなければいけません。
"exclusiveMaximum"が与えられている場合、"maximum"も記述されていなければなりません。

#### バリデーションが成功する場合の条件

"exclusiveMaximum"の有無やその値によって検証方法が変わります。

* "exclusiveMaximum"が与えられていない場合もしくはfalseが与えられている場合
"maximum"以下の値であれば正常です。

* "exclusiveMaximum"にtrueが与えられている場合
"maximum"より小さい値なら正常値です。

#### デフォルト値
"exclusiveMaximum"が与えられていない場合、falseが与えられているものとして考えることができます。

### minimum and exclusiveMinimum

#### 正常値

"minimum"は数値でなければいけません。
"exclusiveMinimum"は真偽値でなければいけません。
"exclusiveMinimum"が与えられている場合、"minimum"も記述されていなければなりません。

#### バリデーションが成功する場合の条件

"exclusiveMinimum"の有無やその値によって検証方法が変わります。

* "exclusiveMinimum"が与えられていない場合もしくはfalseが与えられている場合
"minimum"以上の値であれば正常です。

* "exclusiveMinimum"にtrueが与えられている場合
"maximminimumum"よりお奇異値なら正常値です。

#### デフォルト値
"exclusiveMinimum"が与えられていない場合、falseが与えられているものとして考えることができます。

## 文字列の場合 - Validation keywords for strings

### maxLength

#### 正常値
このキーワードの値は0もしくは自然数でなければなりません。
#### バリデーションが成功する場合の条件
文字列長がこのキーワードの値以下ならば正常値です。
文字列長はRFC4627にしたがってその文字の個数によって定められます。

### minLength

#### 正常値
このキーワードの値は0もしくは自然数でなければなりません。
#### バリデーションが成功する場合の条件
文字列長がこのキーワードの値以上ならば正常値です。
文字列長はRFC4627にしたがってその文字の個数によって定められます。

#### デフォルト値
"minLength"が与えられない場合は0が与えられているものと考えられるでしょう。

### pattern

#### 正常値
この値は文字列でなければなりません。
この文字列はECMA262に準拠した正規表現の表記による、正常な正規表現であるべきです。

#### バリデーションが成功する場合の条件
文字列値が正規表現にマッチする場合に正常値となります。
正規表現が暗黙的に文頭・文末に固定されていないことに注意してください

## 配列の場合 - Validation keywords for arrays

### additionalItems and items
#### 正常値
"additionalItems"は真偽値もしくはオブジェクトのどちらかであるべきです。
オブジェクトである場合、JSON Schemaにしたがってなければなりません。

"items"はオブジェクトもしくは配列のどちらかであるべきです。
もしオブジェクトならば正しいJSON Schemaにしたがってなければなりません。
もし配列の場合はオブジェクトの纏まりで、そしてそれぞれの要素は正しいJSON Schemaにしたがってなければなりません。

#### バリデーションが成功する場合の条件
２つのキーワードに関する配列の値のバリデーションは次に示されるように定められます。
* "items"が与えられていないもしくはobjectである場合、"additionalItems"の値に関わらずバリデーション結果は常に成功です。
* "additionalItems"がtrueもしくはobjectである場合、バリデーション結果は常に成功です。
* "additionalItems"が真偽値でfalseの場合でなおかつ"items"が配列である場合、"items"のサイズ以下であれば正常値です。
#### 例

次の例はバリデーションが唯一失敗する状況である
"additionalItems"がfalseで"items"が配列の場合を補足するための例です。
これはSchemaの例です。

```
   {
       "items": [ {}, {}, {} ],
       "additionalItems": false
   }
```

上記Schemaに基づく正常値は次の通りです。

```

      [] (an empty array),
      [ [ 1, 2, 3, 4 ], [ 5, 6, 7, 8 ] ],
      [ 1, 2, 3 ];
```

また、上記Schemaに基づく不正値は次の通りです

```
      [ 1, 2, 3, 4 ],
      [ null, { "a": "b" }, true, 31.000002020013 ]
```

#### デフォルト値
何も与えられていない場合はどちらも空のSchemaが与えられていると考えられるでしょう。

### maxItems

#### 正常値
このキーワードの値は０もしくは自然数でなければなりません。
#### バリデーションが成功する場合の条件
配列の要素数が"maxItems"の値以下であれば配列の値は正常値です。

### minItems

#### 正常値
このキーワードの値は０もしくは自然数でなければなりません。

#### バリデーションが成功する場合の条件
"minItems"の値よりも配列のサイズが大きければ、その配列は正常値です。

#### デフォルト値
何も与えられていない場合、0が与えれられていると考えられるでしょう。

### uniqueItems

#### 正常値
このキーワードの値は真偽値でなければなりません。

#### バリデーションが成功する場合の条件
このキーワードの値がfalseならば配列の値は正常です。
もし、trueが与えられているならば、それぞれの要素がuniqueである場合にのみ正常です。

#### デフォルト値
何も与えられていない場合はfalseとして考えられるでしょう。

## objectの場合 - Validation keywords for objects

### maxProperties

#### 正常値
このキーワードの値は0もしくは自然数でなければなりません。

#### バリデーションが成功する場合の条件
オブジェクトのプロパティ数がこのキーワードで与えられた値以下であるなら
そのオブジェクトは正常値です。

### minProperties

#### 正常値
このキーワードの値は0もしくは自然数でなければなりません。

#### バリデーションが成功する場合の条件
オブジェクトのプロパティ数がこのキーワードで与えられた値以上であるなら
そのオブジェクトは正常値です。

#### デフォルト値
何も与えられていない場合、0であると考えられるでしょう。

### required

#### 正常値
このキーワードの値は配列でなければなりません。
この配列は一つの値は持っているべきです。
配列の各要素は文字列でなおかつ一意でなければなりません。

#### バリデーションが成功する場合の条件
オブジェクトはこのキーワードの配列の要素全てのプロパティを持っていれば正常値です。

### additionalProperties, properties and patternProperties

#### 正常値
"additionalProperties"の値は配列もしくはオブジェクトでなければなりません。
また、オブジェクトである場合はJSON Schemaに従ってなければなりません。

"properties"の値はオブジェクトでなければなりません。
オブジェクトのメンバそれぞれは全てオブジェクトで、それぞれのオブジェクトは
正しいJSON Schemaでなければなりません。

"patternProperties"の値はオブジェクトでなければなりません。
このオブジェクトのプロパティ名はECMA262に則った正しい正規表現であるべきです。
それぞれのプロパティの値はオブジェクトでなければならず、それぞれの値は正しいJSON Schemaでなければなりません。

#### バリデーションが成功する場合の条件
"additionalProperties"の値によるオブジェクトに対してバリデーションは次のように定められます。

* その値がtrueもしくはschemaである場合は正常値です。
* falseならば下記に示されるアルゴリズムによって決定するバリデーション結果に応じて正常値かどうかが決定します。

#### デフォルト値
"properties"もしくは"patternProperties"が空ならば、空のオブジェクトが与えられていると考えられるでしょう。

"additionalProperties"が空ならば、値としては空のSchemaが与えられるでしょう。

#### additionalPropertiesがfalseの場合
このケースではバリデーションは"properties"もしくは"patternProperties"の値によって定められます。
この章では"parentProperties"のプロパティ名は利便性のための正規表現と呼びます。

まずはじめに次の情報を収集します。
* s 検証すべきプロパティ名のセット
* p "properties"から得られるプロパティ名のセット
* pp "patternProperties"から得られるプロパティ名のセット

これらを用いて次の処理を行います。
* "s"から"p"の要素を全て削除します
* "pp"のそれぞれの正規表現にマッチする全ての要素を"s"から削除します。
上記処理により"s"の中身が空になったならば正常値です。

### サンプル

次のスキーマをサンプルとして使います。

```
{
    "properties": {
        "p1": {}
    },
    "patternProperties": {
        "p": {},
        "[0-9]": {}
    },
    "additionalProperties": false
}
```

次がバリデーションする値です。

```
{
    "p1": true,
    "p2": null,
    "a32&o": "foobar",
    "": [],
    "fiddle": 42,
    "apple": "pie"
}
```

３つのプロパティセットが次のようになります。

```
s  [ "p1", "p2", "a32&o", "", "fiddle", "apple" ]
p  [ "p1" ]
pp [ "p", "[0-9]" ]
```

アルゴリズムの２処理を適用します。

1段階目の処理で"s"から"p1"を取り除かれます
次の処理で"p2" ("p"にマッチ)、"a32&o" ([0-9]にマッチ)、"apple" ("p"にマッチ)が取り除かれます。
この時点で"s"には2つの値が含まれています。(""と"fiddle"です)
そのためバリデーションは失敗となります。

### dependencies

#### 正常値

このキーワードに与えられる値はオブジェクトでなければなりません。
また、オブジェクトのそれぞれの値はオブジェクトもしくは配列でなければなりません。

オブジェクトである場合、正常なJSON Schemaでなければなりません。これはSchema Dependencyと呼ばれます。

配列であるならば一つ以上の要素を持っていなければなりません。
配列の要素は文字列であるべきで、配列内の要素は一意でなければなりません。
これはProperty Dependencyと呼ばれます。

#### バリデーションが成功する場合の条件

##### Schema Dependency

Schema Dependecyの全てのペア（name, schema）に対して、この名前によって与えられるプロパティを持っているならこのSchemaに対して正常であるといえます。

これは与えられた値自身について検証されるべきでプロパティ名とそれに紐づく値のことではないことに留意してください。

##### Property Dependency

Property Dependencyの全てのペア(name, propertyset)に対して
（？）与えられたオブジェクトはこの名前によるプロパティを持っているならば、
（？）プロパティセットと同じ名前を持つプロパティを持っていなければなりません。

## その他型におけるキーワード - Validation keywords for any instance type

### enum

#### 正常値
このキーワードの値は配列でなければなりません。
配列には一つ以上の値が含まれている必要があります。
また、要素は一意でなければなりません。

配列内の要素はヌルを含む他の型かもしれません。

#### バリデーションが成功する場合の条件
与えられた値がこのキーワードの値の要素の一つに一致していれば正常値です。

### type

#### 正常値

このキーワードの値は文字列か配列でなければいけません。
配列の場合、要素は全て文字列で一意でなければいけません。

文字列はCore仕様に則ったプリミティブ型の一つでなければなりません。

#### バリデーションが成功する場合の条件

与えられた値の型がキーワードによって定義された型の一つあれば正常値です。
”number”は"integer"を含むことに注意してください。

### allOf

#### 正常値

このキーワードの値は配列でなければなりません。また、要素は一つ以上持つ必要があります。
要素はオブジェクトでなければなりません。それぞれのオブジェクトは正しいJSON Schemaである必要があります。

#### バリデーションが成功する場合の条件

このキーワードの値で定義される全てのSchemaを満たす場合、正常値です。

### anyOf

#### 正常値
このキーワードの値は配列でなければなりません。また、要素は一つ以上持つ必要があります。
要素はオブジェクトでなければなりません。それぞれのオブジェクトは正しいJSON Schemaである必要があります。

#### バリデーションが成功する場合の条件
このキーワードの値で定義されるいずれかのSchemaを満たす場合、正常値です。

### oneOf

#### 正常値
このキーワードの値は配列でなければなりません。また、要素は一つ以上持つ必要があります。
要素はオブジェクトでなければなりません。それぞれのオブジェクトは正しいJSON Schemaである必要があります。

#### バリデーションが成功する場合の条件
このキーワードの値で定義される一つのSchemaのみを満たす場合、正常値です。

### not

#### 正常値
このキーワードの値はオブジェクトでなければなりません。それぞれのオブジェクトは正しいJSON Schemaである必要があります。

#### バリデーションが成功する場合の条件
このキーワードで与えられるSchemaを満たさない場合、正常値です。

### definitions

#### 正常値
このキーワードの値はオブジェクトでなければなりません。オブジェクトのメンバは正しいJSON Schemaである必要があります。

#### バリデーションが成功する場合の条件

このキーワード単体ではバリデーションには何の役割を持ちません。
これはSchemaの著者のためにより一般的なスキーマへのインラインSchemaを標準座標を提供する役割を持ちます。

例えば、以下に自然数の配列を示した配列のSchemaがありますが、自然数の制約はdefinitionsにサブSchemaとして存在します。

```
{
    "type": "array",
    "items": { "$ref": "#/definitions/positiveInteger" },
    "definitions": {
        "positiveInteger": {
            "type": "integer",
            "minimum": 0,
            "exclusiveMinimum": true
        }
    }
}
```


# メタデータに関するキーワード - Metadata keywords

## "title" and "description"

### 正常値

これらのキーワードの値は文字列でなければなりません。

### 狙い - Purpose

これらの２つのキーワードはユーザーインターフェースによって生み出されるデータについての情報を付記するために用いることが可能です。
titleについては短い方が望ましく, descriptionについてはこのSchemaによって記される
値についての目的を説明を提供するべきです。

これらのキーワードはroot Schemaおよびsub Schemaにおいて使うことが可能です。

## デフォルト - "default"

### 正常値

このキーワードの値には何の制約もありません。

### 狙い - Purpose

このキーワードは部分的なSchemaと一緒に連携してデフォルトのJSONの値を提供される時に用いられます。
関連するSchemaに対して正常な値をデフォルト値として設定することが望まれます。

# 形式を持つセマンティックバリデーション(?) - Semantic validation with "format"

## 前置き - Foreword

構造的検証単独では値がアプリケーションの要求の全てを満たしているかどうかを検証するには不十分かもしれません。
（？）"format"では権威あるリソースによって正確に示されたものやそれらRFCもしくはその他外部の仕様によって
固定されたデータのための意味論的検証の相互運用を満たすために定義されます。

このキーワードに与えられた値はformat attributeと呼ばれます。この値は文字列でなければなりません。
（？）fotmat attributeは一般的に唯一の値の型のセットを検証することが可能です。
値の型にこのセットになければ、このformat attributeによるバリデーションおよび値は正常です。

## 実装要件 - Implementation requirements

実装はこの"format"を実装するかもしれません。
その時次を選ぶべきです。
* 下に定義される属性のバリデーションを実装すべきです。
* このキーワードのバリデーションはオプションによって無効にできるべきです。

実装には独自実装のformat attributesを追加するかもしれません。
当事者間の合意なしに、Schemaの著者はこのキーワードのサポートないし
独自実装のformat attributesについて期待するべきではありません。

## 定義されている属性 - Defined attributes

### date-time

#### 適応性

この属性は文字列に適応されます。

#### バリデーション

[RFC 3339 section 5.6](https://tools.ietf.org/html/rfc3339#section-5.6)によって定義された
正しい日付表現であれば正常値です。

### email

#### 適応性

この属性は文字列に適応されます。

#### バリデーション

[RFC5322 section 3.4.1](https://tools.ietf.org/html/rfc5322#section-3.4.1)によって定義された
正しいInternet email addressであれば正常値です。

### hostname

#### 適応性

この属性は文字列に適応されます。

#### バリデーション

[RFC1034 section3.1](https://tools.ietf.org/html/rfc1034#section-3.1)によって定義された
正しいInternet host nameであれば正常値です。

### ipv4

#### 適応性

この属性は文字列に適応されます。

#### バリデーション

[RFC2673 section3.2](https://tools.ietf.org/html/rfc2673#section-3.2)によって定義された
"dotted-quad" ABNF記法による、正しいIPv4であれば正常値です。
### ipv6


#### 適応性

この属性は文字列に適応されます。

#### バリデーション

[RFC2373 section2.2](https://tools.ietf.org/html/rfc2373#section-2.2)によって定義された
正しいIPv6 addres表記であれば正常値です。

### uri

#### 適応性

この属性は文字列に適応されます。

#### バリデーション

[RFC3986](https://tools.ietf.org/html/rfc3986)によって定義された
正しいURIであれば正常値です

# 内部構造特定のための参照アルゴリズム - Reference algorithms for calculating children schemas
## 前置き - Foreword
## 配列要素 - Array elements
### 定義されている特徴 - Defining characteristic
### 影響されるキーワードとデフォルト値について - Implied keywords and default values
### 型定義方法 - Calculation
## Objectの要素 - Object members
### 定義されている特徴 - Defining characteristic
### 影響されるキーワード - Implied keywords
### 型同定方法 -Calculation

==============================================================

# Security considerations
    [original](https://tools.ietf.org/html/draft-fge-json-schema-validation-00#section-9)
# IANA Considerations
    [original](https://tools.ietf.org/html/draft-fge-json-schema-validation-00#section-10)
# References
    [original](https://tools.ietf.org/html/draft-fge-json-schema-validation-00#section-11)
# Appendix A.  ChangeLog
    [original](https://tools.ietf.org/html/draft-fge-json-schema-validation-00#appendix-A)
