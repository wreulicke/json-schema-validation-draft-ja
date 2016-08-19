JSON Schemaの規格の邦訳
====================================

JSON Schemaの普及のためにこのリポジトリは作成されました。[WIP]

Abstract

JSON Schema (application/schema+json) has several purposes, one of which is instance validation. The validation process may be interactive or non interactive. For instance, applications may use JSON Schema to build a user interface enabling interactive content generation in addition to user input checking, or validate data retrieved from various sources. This specification describes schema keywords dedicated to validation purposes.

Status of This Memo

This Internet-Draft is submitted in full conformance with the provisions of BCP 78 and BCP 79.

Internet-Drafts are working documents of the Internet Engineering Task Force (IETF). Note that other groups may also distribute working documents as Internet-Drafts. The list of current Internet-Drafts is at http://datatracker.ietf.org/drafts/current/.

Internet-Drafts are draft documents valid for a maximum of six months and may be updated, replaced, or obsoleted by other documents at any time. It is inappropriate to use Internet-Drafts as reference material or to cite them other than as “work in progress.”

This Internet-Draft will expire on August 3, 2013.

Copyright Notice

Copyright (c) 2013 IETF Trust and the persons identified as the document authors. All rights reserved.

This document is subject to BCP 78 and the IETF Trust's Legal Provisions Relating to IETF Documents (http://trustee.ietf.org/license-info) in effect on the date of publication of this document. Please review these documents carefully, as they describe your rights and restrictions with respect to this document. Code Components extracted from this document must include Simplified BSD License text as described in Section 4.e of the Trust Legal Provisions and are provided without warranty as described in the Simplified BSD License.

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
        * Implied keywords and default values.
        * Calculation
    * Object members
        * Defining characteristic
        * Implied keywords
        * Calculation
* IANA Considerations
* References
    * Normative References
    * Informative References