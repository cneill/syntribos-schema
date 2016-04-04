# Syntribos Test Schema

## About

This is a place for us to discuss the schema of a Syntribos "Test Suite" / 
"Test Case" / "Issue"

## Abstract

__Finding__

- [Target](#target)
- [Endpoint](#endpoint)
- [Defect Type](#defect-type)
- [Test Type](#test-type)
- [Impacted Parameter](#impacted-parameter)
    - HTTP Method
    - Location
    - Name
    - Value
    - Payload Type
- [Description](#description)
- [Remediation](#remediation)
- [CWE](#cwe)
- [Reference ID](#reference-id)
- Request/Response (?)
    - *ccneill:* need to decide if we really want/need this
- [Severity](#severity)
- [Confidence](#confidence)
- [Impact](#impact)

__Input parameter__

### Target

A hostname/IP/etc. to be tested. This is also tied to a set of request
templates by the way syntribos collects these templates (from a single folder
specified at runtime)

__Current State:__

This is called an "endpoint" today, which is too ambiguous.

__Examples:__

- staging.product.com
- prod.product.com
- 127.0.0.1

__Questions:__

- What is the most accurate name for this?
- Does this include port number?
    - *ccneill:* initial thought, yes

### Endpoint

A specific REST API method, i.e. a URL path associated with a Target

__Current State:__

This doesn't currently have a name (I don't think), but is loosely associated
with a "payload." There may be multiple request templates (what we are
_currently_ calling "payloads") that utilize the same endpoint.

__Example:__

- /v1/lol.json
- /v1/test

### Defect Type

The type of vulnerability that Syntribos believes it has found. This may
be something like 500 error or DoS, regardless of what the Test Type is.

__Example:__

- 500 error
- SQL injection
- Error (connectivity, etc.)

### Test Type

The type of vulnerability that is being tested for. This is not necessarily
the same as the Defect Type, which may be something like 500 error or DoS.

__Current State:__

Not sure if this currently has a single name, but it is represented by strings
like "xml_entities" and "sql_injection" in the `Issue` object

__Examples:__

- XML External Entities Injection
- SQL Injection
- Denial of Service

### Description

__Example:__

- This request may have generated a response indicating vulnerability to
  cross-site scripting

### Remediation

__Examples:__

- Ensure that SSL is enabled on this endpoint
- Ensure that user-controlled variables are properly encoded for output

### CWE

[Common Weakness Enumeration ID](https://cwe.mitre.org/)

__Example:__

- CWE-78: Improper Neutralization of Special Elements used in an OS Command

### Reference ID

__Questions:__

- *ccneill:* Where does this come from? Is it a dotted notation where every
  element is preserved as part of the string, or is it a UUID/similar that is
  generated in a deterministic way, but that is meaningless by itself?

### Severity

__Examples:__

- Critical
- High
- Medium
- Low
- Info

### Confidence

__Examples:__

- High
- Medium
- Low

### Impact

__Examples:__

- Lack of SSL could allow an attacker to MITM clients
- Stack traces can be used to find sensitive information

---

## Impacted Parameter

### Method

The HTTP method used in the test 

__Examples:__

- GET
- POST
- PATCH

__Questions:__

- Do we include this at the "test suite" or "test class" level?
    - *ccneill:* initial thought: test case, but need to figure out how

### Location

The location of the impacted parameter

__Examples:__

- URL resource
- Query string
- HTTP Header
- Body

__Questions:__

- How should we associate this with the ACTION_PARAM?
    - *ccneill:* potential example: [POST:q] OR maybe some crazy JSON thing

### Name

The parameter (e.g. HTTP header, GET var) that was modified by a given test case

__Current State:__

This is untracked currently; we modify the request template on-the-fly and don't
save all the parameters.

__Examples:__

- Content-Type
- "name"

__Questions:__
- Should this be "test case" or "test suite" level?
    - *ccneill:* initial thought: test case

### Value

The "fuzz" string that was supplied in a given test case

__Current State:__

Currently, we call these the "data" or "fuzz strings" or just "strings." The
term "payload" is more common parlance for these strings.

__Examples:__

- `";alert(1)`
- `AAAAAAAAAAAAAAAAA ...`

__Questions:__

- How can we make these easy to reference without including the full string in
  the results?
    - Do we even need the string? Can we just call it "string 4"?
- Do we want to provide this in the "actionable results" log, or just in the
  "debug" log? Both?

### Payload Type

The type of a body (POST/PATCH/etc.) variable.

__Examples:__

- XML
- JSON
- YAML

__Questions:__

- *ccneill*: How do we address these types? Human-readable dotted string, or
  deterministic, but unintelligible, ID?


-----

## Questions

0. How do we want to handle redirects? Are they a common enough problem to
   warrant a piece of metadata in all logs, or just fail logs, or none?
0. How can we solve for URL fuzzing?
    - We can potentially include this info in the request template
    - E.g.: {id:12345} OR {id: get_rand_id} (name:default OR name:generator)
0. How do we handle multiple "findings" for a test. Ex: SQLi triggers DoS, 500,
   and SQL error

--------

# OLD/Defunct

## XUnit Example

```xml
<testsuites>
  <testsuite name="{TARGET}|{ENDPOINT}|{TEST_TYPE}">
    <testcase name="{METHOD} - [{ACTION_PARAM_TYPE}:{ACTION_PARAM}]">
      <failure message="{DESCRIPTION}">{LIST OF FAILED PAYLOADS}</failure>
    </testcase>
  </testsuite>
</testsuites>
```

## JSON Example

```json
{
    "testsuites": {
        "{TARGET}|{ENDPOINT}|{TEST_TYPE}": {
            "{METHOD} - {ACTION_PARAM_TYPE}|{ACTION_PARAM}": {
                "failures": [
                    {
                        "description": "{DESCRIPTION}",
                        "payloads": [ "{FAILED PAYLOAD}", ... ]
                    }
                ],
                "errors": [...],
                "successes": [...],
                "skipped": [...]
            }
        }
    } 
}
```
