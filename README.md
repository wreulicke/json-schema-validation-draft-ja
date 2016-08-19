JSON Schemaの規格の邦訳
====================================

JSON Schemaの普及のためにこのリポジトリは作成されました。[WIP]

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
