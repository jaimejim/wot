# Expressing data types in a Thing Description

Thing description allows one to define a type for input and output values
(e.g., valueType). We need to allow for
- simple types and
- composed types (a.k.a. complex types)

The original idea was to base simple types on XML schema datatypes just like RDF
does (e.g., xsd:integer) and composed types based on schema.org.

That said using XSD datatypes seems to implicitly match to a given
representation (e.g., xsd:byte) while still not providing an easy way to limit
ranges or enumerate values without requiring the full power of XML schema.

Goal
- express how JSON payload MUST look like
- do not prohibit mappings to other formats than JSON (e.g., XML)
- enable semantic annotation of type definition

## Note on RDF data types

It is allowed (but not recommended) to associate a literal to any type
definition that has a URI (even without precise semantics). Still, a dedicated
mechanism to define complex types is needed: there is no way to specifiy the
nature of the data type on a literal.

Refering to arbitrary XSD schema is allowed, as long as schema components
define an ID. The other proposed solution was with XSCD, which never reached
the recommendation status, though.

OWL allows restriction on RDF-compatible XSD types. Structured values, such as
JSON objects should be expressed as classes. A simpler type declaration would
be beneficial.

References:
- https://www.w3.org/TR/rdf11-concepts/#section-Datatypes (RDF data types)
- https://www.w3.org/TR/owl2-syntax/#Datatype_Definitions (OWL data types)
- https://www.w3.org/TR/swbp-xsch-datatypes/#sec-userDefined (user-defined XSD data types)

## Other schemas

### Schema.org

Schema.org allow for refering to existing types but introduces
another concept by linking to types defined
elsewhere. Coming up with user-defined types does not seem to be very easy also.

### JSON Schema

Using JSON schema in both cases, simple types and composed
types, seems a reasonable/possible approach.


## Comparison

<table>
  <tr>
    <th>Feature</th>
    <th>JSON Schema</td>
    <th>Schema.org</td>
    <th>Notes</td>
  </tr>
  <tr>
    <td>Community</td>
    <td>?loosely coupled?</td>
    <td>Strong community</td>
    <td>Schema.org vocabulary is shared between different parties (e.g., Bing, Google, Yahoo)</td>
  </tr>
  <tr>
    <td>Tool support for validation</td>
    <td>Yes e.g.  http://jsonschemalint.com/</td>
    <td>Partially, e.g. https://developers.google.com/structured-data/testing-tool/</td>
    <td>Google's tool only check against existing classes and data types.</td>
  </tr>
  <tr>
    <td>Value constraints</td>
    <td>
      <pre>
{
  "type": "integer",
  "maximum": 13
}
      </pre>
    </td>
    <td>
      <pre>
{
  "@type": "Integer",
  "maxValue": 13
}
      </pre>
    </td>
    <th></td>
  </tr>

  <tr>
    <td>Enumerated value support</td>
    <td>
      <pre>
{
   "type": "string",
   "enum": [ "file", "memory"]
}
      </pre>
    </td>
    <td>
      <i>Not applicable</i>
    </td>
    <td></td>
  </tr>

  <tr>
    <td>User-defined types</td>
    <td>
      <pre>
{
    "type": "object",
    "properties": {
        "id": {
            "type": "integer"
        },
        "name": {
            "type": "string"
        }
    },
    "required": ["id"]
}
      </pre>
    </td>
    <td>
      <pre>
[
  {
    "@type": "PropertyValueSpecification",
    "valueName": "id",
    "valueRequired": true,
  },
  {
    "@type": "PropertyValueSpecification",
    "valueName": "name"
  }
]
      </pre>
    </td>
    <td>A request to the schema.org community is needed to extend its model</td>
  </tr>
</table>

## Next steps

JSON Schema appears a good candidate and it is recommended for complex data
type specification. However, it still needs to be mapped to RDF, so that type
definitions can be semantically annotated.

Moreover, further languages have been proposed
([CDDL](https://tools.ietf.org/html/draft-greevenbosch-appsawg-cbor-cddl-08),
[YANG](https://tools.ietf.org/html/draft-ietf-netmod-rfc6020bis-12))
that still need to be reviewed.
