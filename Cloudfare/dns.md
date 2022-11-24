## IP address resolution

At least one IP address resolution record is required for each domain on Cloudflare.
These records are the only ones you can proxy through Cloudflare.

### Record Types

By default, Cloudflare only supports proxied A, AAAA, and CNAME records. You cannot proxy other record types.<br>

<li>    A and AAAA records map a domain name to one or multiple IPv4 or IPv6 address(es).
<li>    CNAME records map a domain name to another (canonical) domain name. Can be used to resolve other record types present on the target domain name.
<pre>
Type	Name	IPv4 address	Proxy status
A       *.www   192.0.2.2       Proxied
</pre>

<li> TTL: Time to live, which controls how long DNS resolvers should cache a response before revalidating it.
If the Proxy Status is Proxied, this value defaults to Auto, which is 300 seconds.
If the Proxy Status is DNS Only, you can customize the value.

#### Proxied records

When an A, AAAA, or CNAME record is Proxied — also known as being orange-clouded — DNS queries for these will resolve to Cloudflare Anycast IPs instead of their original DNS target.
This means that all requests intended for proxied hostnames will go to Cloudflare first and then be forwarded to your origin server.
This behavior allows Cloudflare to optimize, cache, and protect all requests to your application, as well as protect your origin server from DDoS attacks.
Because requests to proxied hostnames go through Cloudflare before reaching your origin server, all requests will appear to be coming from Cloudflare’s IP addresses (and could potentially be blocked or rate limited). If you use proxied records, you may need to adjust your server configuration to allow Cloudflare IPs.

#### DNS-only records

When an A, AAAA, or CNAME record is DNS-only — also known as being gray-clouded — DNS queries for these will resolve to the record’s normal IP address.

In addition to potentially exposing your origin IP addresses to bad actors and DDoS attacks, leaving your records as DNS-only means that Cloudflare cannot optimize, cache, and protect requests to your application.
