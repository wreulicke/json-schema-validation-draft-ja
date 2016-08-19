JSON Schemaの規格の邦訳
====================================

JSON Schemaの普及のためにこのリポジトリは作成されました。[WIP]

# 概要

JSON Schema (application/schema+json)には様々な目的があり、その一つがデータのバリデーションです。
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

* Introduction
* Conventions and Terminology
* Interoperability considerations
    * Validation of string instances
    * Validation of numeric instances
    * Regular expressions
* General validation considerations
    * Keywords and instance primitive types
    * Inter-dependent keywords
    * Default values for missing keywords
    * Validation of container instances
* Validation keywords sorted by instance types
    * Validation keywords for numeric instances (number and integer)
        * multipleOf
        * maximum and exclusiveMaximum
        * minimum and exclusiveMinimum
    * Validation keywords for strings
        * maxLength
        * minLength
        * pattern
    * Validation keywords for arrays
        * additionalItems and items
        * maxItems
        * minItems
        * uniqueItems
    * Validation keywords for objects
        * maxProperties
        * minProperties
        * required
        * additionalProperties, properties and patternProperties
        * dependencies
    * Validation keywords for any instance type
        * enum
        * type
        * allOf
        * anyOf
        * oneOf
        * not
        * definitions
* Metadata keywords
    * "title" and "description"
        * Valid values
        * Purpose
    * "default"
        * Valid values
        * Purpose
* Semantic validation with "format"
    * Foreword
    * Implementation requirements
    * Defined attributes
        * date-time
        * email
        * hostname
        * ipv4
        * ipv6
        * uri
* Reference algorithms for calculating children schemas
    * Foreword
    * Array elements
        * Defining characteristic
        * Implied keywords and default values
        * Calculation
    * Object members
        * Defining characteristic
        * Implied keywords
        * Calculation
* IANA Considerations
* References
    * Normative References
    * Informative References
