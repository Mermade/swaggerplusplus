# swaggerplusplus

### A proposal for transitioning between Swagger 2.0 and OpenAPI 3.0.x

To aid the transition between Swagger 2.0 and OpenAPI 3.0.x, it is proposed to back-port the following features from the OpenAPI 3.0.0 specification (currently 3.0.0-RC0 implementor's draft).

Tooling may make use of these features now, with minimal work required to support them in OpenAPI 3.0.x, definition authors may also use these features, knowing their API definition can be losslessly converted from **swaggerplusplus** to OpenAPI 3.0.x by any **swaggerplusplus**-compatible converter.

## Features

Path|Object|New Extension|Description
---|---|---|---|
#/|servers|x-servers|An array of [Server Objects](https://github.com/OAI/OpenAPI-Specification/blob/OpenAPI.next/versions/3.0.md#server-object)
\*|anyOf|x-anyOf|Schema must be extracted and post-processed before validation
\*|oneOf|x-oneOf|Schema must be extracted and post-processed before validation
\*|not|x-not|Schema must be extracted and post-processed before validation
\*|required|x-original-required|Where a property has been removed from `required` due to use of `x-anyOf`, `x-oneOf` or `x-not`

## Tools

* [Converter/validator](https://github.com/mermade/swagger2openapi)
* [Rebilly Redoc](https://github.com/Rebilly/ReDoc) - supports a subset of **swaggerplusplus**
