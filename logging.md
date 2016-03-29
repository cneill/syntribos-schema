# Syntribos Logging

## About

This is a place for us to discuss specifics of Syntribos' log output (e.g.
output formatting, naming, verbosity, etc.)

-----

## Questions

0. How to structure XUnit/JSON "results" log (testcase/testsuite names/attributes/etc.)
0. What to put in the "results" log vs. "debug" log. Do we need both?
0. Should we just add a "playback" function and do away with the debug log for
   ALL tests?
    - Save a reversible identifier that lets us trace back and re-generate the
      original request
    - Have Syntribos read the XUnit/JSON "results" log, and have it re-run the
      "failed" tests and save more detail, but only if you ask for it by running
      a second "detail" or "verification" command
        - You could also specify specific requests, maybe like so:
      `syntribos poc SQL_HEADER_PAYLOAD_1_TARGET_1_ENDPOINT_1` or similar, and
      it would make that exact request again, and allow you to save more details
      (like what our debug log does today for ALL requests)
0. Can there be a options for changing the severity of tests, from testing for critical issues to low/detailed, or some way in which the user get realtime status of tests? 
0. Is there any thoughts on giving the test results in yaml as well, so its is in a way both machine readable as well as more verbose than json.? 

