# What is 'ggdata'?

This repo, 'ggdata' is a *thoroughly unofficial* companion command-line interface (CLI) to the GitGuardian platform tooling ['ggshield'](https://github.com/GitGuardian/ggshield). It is a prototype data exporter that facilitates use cases such as Tableau reporting, custom Honeytoken alerting workflows, and archiving of incident remediation steps.

# Why does 'ggdata' exist?

This exists as a separate tool from ggshield instead of a fork with additions because A) it is an unofficial utility and B) because the GitGuardian team is extremely skilld, so incorporating it into whatever place in their offerings is purely up to them. This might eventually include their official platform, but likely will remain a separate tool in order to iterate quickly based on user feedback.

# TL;DR How to use 'ggdata'

By default, ggdata limits reported data to *reasonable* amounts, primarily based on severity and timeframe defaults, since this is the most obvious and requested mode. Only when requested will it dig deeper using other filters in order to remain economical in its use of API calls.

## Export Core Incident Data

'''ggdata secret incidents'''

## Export Incident Data with Team Perimeter Grouping

'''ggdata secret incidents --include-teams'''

# (Optional) Default Data Format

By default, the above commands produce JSON data. This is useful for piping to utilities like 'jq' or other external systems.

Alternatively, you can specify an output format using the '-o' or '--output' commands and specify a value of 'CSV' to obtain flattened top-level field data.

# (Optional) Templating and Default 'Pretty' Printing

## Console Pretty Printing

If you intent to use ggdata for console/command-line human interactions, you can set the global variable 'GGDATA_OUTPUT=CLI' which will use built-in templates to produce 'pretty' outputs to stdout, such as tables and other optimal formats for real-time status results.

## Report Templates

You can also use built-in templates or specify your own Mustache templates to format the JSON data before the CLI returns output to stdout. To do so, use the --report parameter. For instance:

'''ggdata --report={enum|filepath} secret incidents'''

Enum: TABLEAU,GSHEETS,XLSX,XML

Note: if using a filepath, this should be an absolute path or relative to the current directory.

Many customers tend to use the custom template to specify a Mustache HTML-based template which results in a single-file report, easily archivable and connected to a CI pipeline run or issue tracking ticket.

# (Optional) Filtering

Most of the following additional parameters can be used with the '...secret incidents' subcommands as well as eventually 'honeytoken...' commands.

Note: Multiple uses of operators are computed from left to right, not grouped by terms. In the future, it may be possible to group terms.

## Filter by Status

'''... --status={string||string&&string&&!string}'''

Enum: IGNORED TRIGGERED ASSIGNED RESOLVED

## Filter by Severity

'''... --severity={string||string&&string&&!string}'''

Enum: critical high medium low info unknown

## Filter by Date Range

'''... --before={string <datetime>}'''

Example: --before=2019-08-30T14:15:22Z

'''... --after={string <datetime>}'''

Example: --after=2019-08-22T14:15:22Z

Note: if before or after parameters are omitted, they do not get passed to the underlying API calls. Thus, for instance, using *--after* alone only produces data AFTER the datetime provided.

## Filter by Tags

'''... --tags={string,string,string,...}'''

Enum: DEFAULT_BRANCH FROM_HISTORICAL_SCAN CHECK_RUN_SKIP_FALSE_POSITIVE CHECK_RUN_SKIP_LOW_RISK CHECK_RUN_SKIP_TEST_CRED PUBLIC PUBLICLY_EXPOSED PUBLICLY_LEAKED REGRESSION SENSITIVE_FILE TEST_FILE NONE

## Filter by Detector Group

'''... --detector-group={string}'''

Example: --detector-group=slackbot_token

# Future Improvements

Even before ggdata is adopted by thousands of teams, there are a few likely improvements that could be made:

- align with ggshield actions, such as:
    - 'auth' and 'logout' commands
    - honeytoken commands, including additional fringe case usage
    - 'Has My Secret Leaked' (hmsl) data

# Contributing

If you would like to contribute, first reach out to GitGuardian either through your assign technical account manager, account executive, or sales engineering professional. Unlike a support ticket for issues, your improvement and contribution inquiries can be best routed and addressed through these mechanisms.

If you still feel like contributing to this repository, fork it then submit a Pull Request (PR) to the upstream origin and our maintainers will take a look at your changes.

# Who is behind ggdata?

The primary contributor, for now, is [Paul Bruce](https://paulsbruce.io) (the current repo owner). Hopefully GitGuardian customers looking to use their platform data will contribute issues, improvements, and feedback to GitGuardian product management. However, it is very likely that this will