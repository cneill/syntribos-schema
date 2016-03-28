# Syntribos Schema

## About

This is a place for us to collaborate on deciding what results from Syntribos
should look like, including output formats, logging verbosity, finding/issue
schema, etc.

## Table of Contents

0. [Test Schema](./test_schema.md)
0. ["Results" log vs. "Debug" log](./logging.md)
0. [General questions](#questions)

------

# Questions

0. How do we handle multi-failures (e.g. 500 error + SQL injection or XSS)?
    - *ccneill:* Today, if a test fails the 500 check, I don't think Syntribos
      checks the other assertions. We need to figure out a way to classify this
      (e.g. maybe we only return "500 error" AFTER checking for SQL injection/
      etc.)
