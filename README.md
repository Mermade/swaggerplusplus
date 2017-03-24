# swaggerplusplus

### A proposal for transitioning between Swagger 2.0 and OpenAPI 3.0.x

#### Version 1.0.0-RC0

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119.

To aid the transition between Swagger 2.0 and OpenAPI 3.0.x, it is proposed to back-port the following features from the OpenAPI 3.0.0 specification (currently 3.0.0-RC0 implementor's draft) as [specification extensions](https://github.com/OAI/OpenAPI-Specification/blob/OpenAPI.next/versions/2.0.md#vendorExtensions) (formerly known as vendor extensions).

Tooling MAY make use of these features now, with minimal work required to support them in OpenAPI 3.0.x, definition authors MAY also use these features, knowing their API definition can be losslessly converted from **swaggerplusplus** to OpenAPI 3.0.x by any **swaggerplusplus**-compatible converter.

## Features

Path|Object|New Extension|Type|Description
---|---|---|---|---|
#/|servers|x-servers|[[Server Objects](https://github.com/OAI/OpenAPI-Specification/blob/OpenAPI.next/versions/3.0.md#server-object)]|When converting to OpenAPI 3.0.x, this array MUST be **prepended to** any existing `servers` array converted from Swagger 2.0 metadata
#/paths{pathItem}|servers|x-servers|[[Server Objects](https://github.com/OAI/OpenAPI-Specification/blob/OpenAPI.next/versions/3.0.md#server-object)]|
#/paths{pathItem}/{operation}|servers|x-servers|[[Server Objects](https://github.com/OAI/OpenAPI-Specification/blob/OpenAPI.next/versions/3.0.md#server-object)]|
#/paths within [PathItem Object]()/[Operation Object]()|trace|x-trace|[V2 Operation Object]()|This MUST be a Swagger 2.0 Operation Object, it MUST be treated as per any other Operation Object, for the `TRACE` HTTP method
#/parameters OR #/paths/{pathItem}/parameters OR #/paths/{pathItem}/{operation}/parameters|deprecated|x-deprecated|Boolean|Indicates the parameter is deprecated and SHOULD be transitioned out of use
#/paths{pathItem}/{operation}|callbacks|x-callbacks|[[Callback Objects](https://github.com/OAI/OpenAPI-Specification/blob/OpenAPI.next/versions/3.0.md#server-object)]|
\* within [Schema Object](https://github.com/OAI/OpenAPI-Specification/blob/OpenAPI.next/versions/3.0.md#schemaObject)|anyOf|x-anyOf|[[Schema Object](https://github.com/OAI/OpenAPI-Specification/blob/OpenAPI.next/versions/3.0.md#schemaObject)]|Schema MUST be extracted and post-processed before being used for validation
\* within [Schema Object](https://github.com/OAI/OpenAPI-Specification/blob/OpenAPI.next/versions/3.0.md#schemaObject)|oneOf|x-oneOf|[[Schema Object](https://github.com/OAI/OpenAPI-Specification/blob/OpenAPI.next/versions/3.0.md#schemaObject)]|Schema MUST be extracted and post-processed before being used for validation
\* within [Schema Object](https://github.com/OAI/OpenAPI-Specification/blob/OpenAPI.next/versions/3.0.md#schemaObject)|not|x-not|[Schema Object](https://github.com/OAI/OpenAPI-Specification/blob/OpenAPI.next/versions/3.0.md#schemaObject)|Schema MUST be extracted and post-processed before being used for validation
\* within [Schema Object](https://github.com/OAI/OpenAPI-Specification/blob/OpenAPI.next/versions/3.0.md#schemaObject)|required|x-required|Array|Where a property has been removed from `required` due to use of `x-anyOf`, `x-oneOf` or `x-not`, converters MUST merge these arrays when converting from **swaggerplusplus** to OpenAPI 3.0.x. When a converter converts from 3.0.x to **swaggerplusplus** it MUST remove any `required` properties hidden by `x-anyOf`, `x-oneOf` or `x-not` and move them into this array.
#/|components/links|x-links|Map {Link Object}|Contains reusable Link Objects
#/|components/callbacks|x-callbacks|Map {Callback Object}|Contains reusable Callback Objects

## Conversion

Converters MUST validate the specification extensions are of the expected type, and that mandatory properties (if any) are present.

## Tools

* [Converter/validator](https://github.com/mermade/swagger2openapi)
* [Rebilly Redoc](https://github.com/Rebilly/ReDoc) - supports a subset of **swaggerplusplus**

## In the wild

[Zappiti API](http://zappiti.com/api/zappiti-player-4k/swagger/)
