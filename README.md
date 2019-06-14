<center><a href='https://hatebase.org'><img src="logo.png" width="300" ></a></center><br />

Please review this README for a general understanding of the API before proceeding into the version-specific documentation. If you need a basic step-by-step introduction to connecting, authenticating and basic data retrieval, check out [our blog post](https://thesentinelproject.org/2018/12/11/getting-started-with-the-hatebase-api-v4-0/).

# Versions

- [v4.2](current/v4-2/overview.md) **Current**
- [v4.1](archived/v4-1/overview.md) Deprecated
- [v4.0](archived/v4-0/overview.md) Deprecated
- [v3.0](archived/v3-0/overview.md) Retired
- [v2.0](archived/v2-0/overview.md) Retired
- v1.0 Retired

Note that decimal increments are backwards-compatible, whereas integer increments are not. In other words, an integration built for v4.0 will work reliably with v4.1 (albeit without the extra features of 4.1) by simply changing the version number in the URL from 4.0 to 4.1 to avoid accessing the older deprecated version.

# How It Works

The Hatebase API accepts queries using a two-phase approach: the authentication handshake and then the actual query itself. In either scenario, the system is queried with a version number, an endpoint (which defines the type of information requested) and a payload containing input parameters (sent by POST), and then returns a resultset.

The URL structure for querying the API is as follows:

```
https://api.hatebase.org/1-0/authenticate
```

"1-0" represents the version number, and "authenticate" represents the endpoint. All versions and endpoints will be documented here.

# Authentication Phase

The API is queried with a key which is obtained from your Hatebase account:

```
{
  api_key: ABC123
}
```
Using curl this can be done as follows:
```bash
curl -d 'api_key=ABC123' https://api.hatebase.org/1-0/authenticate
```

The returned data provides the input parameters which were queried, plus a resultset (in this case a session token), plus any warnings and/or errors:


```
{
  datetime: gmt_time,
  key: ABC123,
  format: json,
  result: [
    token: DEF456,
    expires_on: "2018-01-01 12:00:00"
  ]
  errors: [
    [789, "Incorrect key"]
  ]
  warnings:[
    [012, "Unspecified format"]
  ]
}
```

Note that this result set is solely illustrative; a token would never be provided if there were errors, only warnings.

Tokens will expire within one hour of issue.

# Query Phase

The token provided in the authentication phase is used as an input parameter to authenticate phase two: the actual data query. The result set will look something like this:


```
{
  datetime: gmt_time,
  token: DEF456,
  expires_on: "2018-01-01 12:00:00",
  format: json,
  page: 1,
  total_pages: 14,
  result: [
  ]
  errors: [
  ]
  warnings:[
  ]
}
```

Again, this simple result set is solely illustrative; there is no need for empty key-value pairs.

# Daily Rate Limiting

Access to the API is limited by your plan's maximum daily query limit. It is strongly recommended that systems which connect with the API do not do so in user real-time. A safer architecture is to download data asynchronously and store locally for better real-time performance and no redundant data retrieval.

# Errors and Warnings

[https://hatebase.org/api_error_codes](https://hatebase.org/api_error_codes)

# Please Contact Us

Although every effort has been made to ensure that this documentation accurately reflects the operation of the API, please advise us if you discover any discrepancies.
