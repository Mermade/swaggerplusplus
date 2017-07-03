# swaggerplusplus

### A proposal for transitioning between Swagger 2.0 and OpenAPI 3.0.x

#### Version 1.0.0-rc1

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [BCP 14](https://tools.ietf.org/html/bcp14) [RFC2119](https://tools.ietf.org/html/rfc2119) [RFC8174](https://tools.ietf.org/html/rfc8174) when, and only when, they appear in all capitals, as shown here.

To aid the transition between Swagger 2.0 and OpenAPI 3.0.x, it is proposed to back-port the following features from the OpenAPI 3.0.0 specification (currently 3.0.0-rc2 implementor's draft) as [specification extensions](https://github.com/OAI/OpenAPI-Specification/blob/OpenAPI.next/versions/2.0.md#vendorExtensions) (formerly known as vendor extensions).

Tooling MAY make use of these features now, with minimal work required to support them in OpenAPI 3.0.x, definition authors MAY also use these features, knowing their API definition can be losslessly converted from **swaggerplusplus** to OpenAPI 3.0.x by any **swaggerplusplus**-compatible converter.

## Features

Path|Version 3.0.x Object|New Extension|Type|Description
---|---|---|---|---|
#/|servers|x-servers|[[Server Objects](https://github.com/OAI/OpenAPI-Specification/blob/OpenAPI.next/versions/3.0.md#server-object)]|When converting to OpenAPI 3.0.x, this array MUST be **prepended to** any existing `servers` array converted from Swagger 2.0 metadata
#/paths/{[Path Item](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#path-item-object)}|servers|x-servers|[[Server Objects](https://github.com/OAI/OpenAPI-Specification/blob/OpenAPI.next/versions/3.0.md#server-object)]|
#/paths/{[Path Item](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#path-item-object)}/{[operation](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#operation-object)}|servers|x-servers|[[Server Objects](https://github.com/OAI/OpenAPI-Specification/blob/OpenAPI.next/versions/3.0.md#server-object)]|
#/paths/{[Path Item](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#path-item-object)}/{[Operation](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#operation-object)}|trace|x-trace|[V2 Operation Object](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#operation-object)|This MUST be a Swagger 2.0 Operation Object, it MUST be treated as per any other Operation Object, for the `TRACE` HTTP method
#/paths/{[Path Item](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#path-item-object)}|summary|x-summary|String|Also as per [Apiary extension](https://help.apiary.io/api_101/swagger-extensions/#x-summary-and-x-description)
#/paths/{[Path Item](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#path-item-object)}|description|x-description|String|Also as per [Apiary extension](https://help.apiary.io/api_101/swagger-extensions/#x-summary-and-x-description)
#/[parameters](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#parameters-definitions-object) **OR** #/paths/{[Path Item](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#path-item-object)}/parameters **OR** #/paths/{[Path Item](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#path-item-object)}/{[Operation](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#operation-object)}/parameters|deprecated|x-deprecated|Boolean|Indicates the parameter is deprecated and SHOULD be transitioned out of use
#/paths/{[Path Item](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#path-item-object)}/[{Operation}](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#operation-object)|callbacks|x-callbacks|[[Callback Objects](https://github.com/OAI/OpenAPI-Specification/blob/OpenAPI.next/versions/3.0.md#server-object)]|
\* within [Schema Object](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#schema-object)|anyOf|x-anyOf|[[Schema Object](https://github.com/OAI/OpenAPI-Specification/blob/OpenAPI.next/versions/3.0.md#schemaObject)]|Schema MUST be extracted and post-processed before being used for validation
\* within [Schema Object](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#schema-object)|oneOf|x-oneOf|[[Schema Object](https://github.com/OAI/OpenAPI-Specification/blob/OpenAPI.next/versions/3.0.md#schemaObject)]|Schema MUST be extracted and post-processed before being used for validation
\* within [Schema Object](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#schema-object)|not|x-not|[Schema Object](https://github.com/OAI/OpenAPI-Specification/blob/OpenAPI.next/versions/3.0.md#schemaObject)|Schema MUST be extracted and post-processed before being used for validation
\* within [Schema Object](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#schema-object)|required|x-required|Array|Where a property has been removed from `required` due to use of `x-anyOf`, `x-oneOf` or `x-not`, converters MUST merge these arrays when converting from **swaggerplusplus** to OpenAPI 3.0.x. When a converter converts from 3.0.x to **swaggerplusplus** it MUST remove any `required` properties hidden by `x-anyOf`, `x-oneOf` or `x-not` and move them into this array.
\* within [Response Object](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#response-object)|links|x-links|Map {[Link Object](https://github.com/OAI/OpenAPI-Specification/blob/OpenAPI.next/versions/3.0.md#link-object)}|Links or references to reusable Link Objects
#/|components/links|x-links|Map {[Link Object](https://github.com/OAI/OpenAPI-Specification/blob/OpenAPI.next/versions/3.0.md#link-object)}|Contains reusable Link Objects
#/|components/callbacks|x-callbacks|Map {[Callback Object](https://github.com/OAI/OpenAPI-Specification/blob/OpenAPI.next/versions/3.0.md#callback-object)}|Contains reusable Callback Objects

## Conversion

Converters MUST validate the specification extensions are of the expected type, and that mandatory properties (if any) are present. Failing that, they MUST leave the specification extension unchanged.

## swaggerplusplus Tools

* [Converter/validator](https://github.com/mermade/swagger2openapi)
* [Rebilly Redoc](https://github.com/Rebilly/ReDoc) - supports a [subset](https://github.com/Rebilly/ReDoc/blob/master/docs/redoc-vendor-extensions.md) of **swaggerplusplus**

## swaggerplusplus in the wild

[Zappiti API](http://zappiti.com/api/zappiti-player-4k/swagger/)
