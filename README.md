# Exometer graphite reporter.

## Usage.

This reporter is meant to be used with
[exometer_core](https://github.com/Feuerlabs/exometer_core).

### exometer\_report\_graphite

The graphite reporter uses the TCP/IP protocol to forward
subscribed-to metrics and data points to a graphite server, such as
the one provided by [`http://hostedgraphite.com`](http://hostedgraphite.com).
When the graphite
reporter receives a metric-datapoint value (subscribed to through
`exometer_report:subscriber()`), the reporter will immediately
forward the key-value pair to the graphite server.

#### Configuring graphite reporter

Below is an example of the a graphite reporter application environment, with
its correct location in the hierarchy:

```erlang

{exometer, [
    {report, [
        {reporters, [
            {exometer_report_graphite, [
                {connect_timeout, 5000},
                {prefix, "web_stats"},
                {host, "carbon.hostedgraphite.com"},
                {port, 2003},
                {api_key, "267d121c-8387-459a-9326-000000000000"}
            ]}
        ]}
    ]}
]}
```

The following attributes are available for configuration:

+ `connect_timeout` (milliseconds - default: 5000)<br />Specifies how long the
graphie reporter plugin shall wait for a tcp connection to complete before
timing out. A timed out connection will not be reconnected to automatically.
(To be fixed.)

+ `prefix` (string - default: "")<br />Specifies an optional prefix to prepend
all metric names with before they are sent to the graphite server.

+ `host` (string - default: "carbon.hostedgraphite.com")<br />Specifies the
name (or IP address) of the graphite server to report to.

+ `port` (integer - default: 2003)<br />Specifies the TCP port on the given
graphite server to connect to.

+ `api_key` (string - default: n/a)<br />Specifies the api key to use when
reporting to a hosted graphite server.

If `prefix` is not specified, but `api_key` is, each metrics will be reported
as `ApiKey.Metric`.

If `prefix` is specified, but `api_key` is not, each metrics will be reported
as `Prefix.Metric`.

if neither `prefix` or `api_key` is specified, each metric will be reported
simply as `Metric`.

## Lineage.

This is a direct copy of the original graphite reporter from
[exometer](https://github.com/Feuerlabs/exometer). In order to use the
graphite reporter in a project with a modern rebar3 based build system, I've
extracted the graphite reporter to avoid issues in exometer's dependencies,
since exometer_core works fine and has a maintained hex.pm package.

Overall project structure stolen from the
[exometer_report_statsd](https://github.com/MyMedsAndMe/exometer_report_statsd)
package and used as an initial skeleton.
