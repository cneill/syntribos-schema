# Syntribos Test Schema

## About

This is a place for us to discuss the schema of a Syntribos "Test Suite" / 
"Test Case" / "Issue"

## Abstract

- Test Suite
    - Target
    - Endpoint
    - Test type
    - List of Test Cases
        - Method
        - Action param
        - Action param type
        - Description of test
        - Failed payloads
        - Failure messages

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
                "successes": [...]
            }
        }
    } 
}
```

### Target / Host

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

### Method

The HTTP method used in the test 

__Examples:__

- GET
- POST
- PATCH

__Questions:__

- Do we include this at the "test suite" or "test class" level?
    - *ccneill:* initial thought: test case, but need to figure out how

### Endpoint

A specific REST API method, i.e. a URL path associated with a Target

__Current State:__

This doesn't currently have a name (I don't think), but is loosely associated
with a "payload." There may be multiple request templates (what we are *currently*
calling "payloads") that utilize the same endpoint.

__Example:__

- /v1/lol.json
- /v1/test

### Test Type

The type of vulnerability that is being tested for 

__Current State:__

Not sure if this currently has a single name, but it is represented by strings
like "xml_entities" and "sql_injection" in the `Issue` object

__Examples:__

- XML External Entities Injection
- SQL Injection
- Denial of Service

### Action Param

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

### Action Param Type

The type of ACTION_PARAM (e.g. HTTP header, GET var)

__Examples:__

- GET variable
- HTTP Header

__Questions:__

- How should we associate this with the ACTION_PARAM?
    - *ccneill:* potential example: [POST:q] OR maybe some crazy JSON thing

### Payload

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

### Description

__Example:__

- This request may have generated a response indicating vulnerability to
  cross-site scripting

-----

## Questions
