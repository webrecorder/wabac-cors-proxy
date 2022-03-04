# wabac.js CORS Proxy

This provides a simple CORS proxy, which is designed to run as a Cloudflare Worker.

This system is compatible with [wabac.js](https://github.com/webrecorder/wabac.js)-based tools, including [ArchiveWeb.page](https://archiveweb.page) and [ReplayWeb.page](https://replayweb.page)

## Custom Headers and Status

### Redirect

The proxy is designed to be usable with regular browser `fetch()`.
Since fetch does not handle redirect requests, the proxy wraps any 3xx response and returns a 200 response with the headers set:


Response headers:
- `x-redirect-status` - redirect status code
- `x-redirect-statusText` - redirect status text
- `x-orig-location` - value of `Location` header

### Cookies / Referer

The proxy also handles custom cookie headers, in case they are not sent/filtered out, such as in service workers

Request header:
- `x-proxy-cookie` - passed as `Cookie` header to upstream server.
- `x-profy-referer` - passed as `Referer` header to upstream server. The `Origin` header is also updated based on value of referer, if it exists.

Response header:
- `x-proxy-set-cookie` - returned from upstream `Set-Cookie` header in case it gets filtered out.

### Usage

Note: CloudFlare `wrangler` cli tool is required. To login, use `wrangler login` to login to your Cloudflare Account.

1. Set the `CORS_ALLOWED_ORIGINS` to a list of allowed urls, or set to null to allow any origin.

2. Deploy with `wrangler publish`

