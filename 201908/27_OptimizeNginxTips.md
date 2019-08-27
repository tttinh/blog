# Optimize NGINX Tips

[Home](../README.md)

Recently, I have a task to update a web service from HTTP to HTTPS. And I noticed that with HTTPS, the APIs became very slow. So I need to reduce the response time. After some research, I found that this web service stay behind an NGINX reverse proxy. And there are something we can do to optimize its performance. That are: keep alive timeout and SSL cache.

## HTTP Keep Alive

HTTP keep-alive, a.k.a., HTTP persistent connection, is an instruction that allows a single TCP connection to remain open for multiple HTTP requests/responses.

By default, HTTP connections close after each request. When someone visits your site, their browser needs to create new connections to request each of the files that make up your web pages (e.g. images, Javascript, and CSS stylesheets), a process that can lead to high page load times (Or high reponse time in case of web services).

Enabling the keep-alive header allows you to serve all web page resources over a single connection. Keep-alive also reduces both CPU and memory usage on your server.

### The Benefits of Connection Keep Alive

The HTTP keep-alive header maintains a connection between a client and your server, reducing the time needed to serve files. A persistent connection also reduces the number of TCP and SSL/TLS connection requests, leading to a drop in round trip time (RTT).

Establishing a TCP connection first requires a three-way handshake – a mutual exchange of SYN and ACK packets between a client and server before data can be transmitted. Using the keep-alive header means not having to constantly perform this process. This results in:

- Network resource conservation – It’s less taxing on network resources to use a single connection per client.
- Reduced network congestion – Reducing the number of TCP connections between your servers and clients can lead to a drop in network       congestion.
- Decreased latency – Reducing the number of three-way handshakes can lead to improved site latency. This is especially true with SSL/     TLS connections, which require additional round-trips to encrypt and verify connections.

## Optimize SSL/TLS

The Secure Sockets Layer (SSL) protocol and its successor, the Transport Layer Security (TLS) protocol, are being used on more and more websites. SSL/TLS encrypts the data transported from origin servers to users to help improve site security. Part of what may be influencing this trend is that Google now uses the presence of SSL/TLS as a positive influence on search engine rankings.

Despite rising popularity, the performance hit involved in SSL/TLS is a sticking point for many sites. SSL/TLS slows website performance for two reasons:

1. The initial handshake required to establish encryption keys whenever a new connection is opened. The way that browsers using HTTP/1.x establish multiple connections per server multiplies that hit.
2. Ongoing overhead from encrypting data on the server and decrypting it on the client.

The mechanism for optimizing SSL/TLS varies by web server. As an example, NGINX uses OpenSSL, running on standard commodity hardware, to provide performance similar to dedicated hardware solutions. NGINX SSL performance is well‑documented and minimizes the time and CPU penalty from performing SSL/TLS encryption and decryption.

Take a look at [blog post](https://www.nginx.com/blog/improve-seo-https-nginx/) for details on ways to increase SSL/TLS performance. To summarize briefly, the techniques are:

- Session caching – Uses the ssl_session_cache directive to cache the parameters used when securing each new connection with SSL/TLS.
- Session tickets or IDs – These store information about specific SSL/TLS sessions in a ticket or ID so a connection can be reused smoothly, without new handshaking.
- OCSP stapling – Cuts handshaking time by caching SSL/TLS certificate information.

## NGINX Configuration

### Keep Alive Connections

Keep alive connections can have a major impact on performance by reducing the CPU and network overhead needed to open and close connections. NGINX terminates all client connections and creates separate and independent connections to the upstream servers. NGINX supports keepalives for both clients and upstream servers. The following directives relate to client keepalives:

- `keepalive_requests` – The number of requests a client can make over a single keepalive connection. The default is 100, but a much higher value can be especially useful for testing with a load‑generation tool, which generally sends a large number of requests from a single client.
- `keepalive_timeout` – How long an idle keepalive connection remains open.

The following directive relates to upstream keepalives:

- keepalive – The number of idle keepalive connections to an upstream server that remain open for each worker process. There is no default value.

To enable keepalive connections to upstream servers you must also include the following directives in the configuration:

```nginx
proxy_http_version 1.1;
proxy_set_header Connection "";
```

### HTTPS Optimization

SSL operations consume extra CPU resources. The most CPU-intensive operation is the SSL handshake. There are two ways to minimize the number of these operations per client:

- Enabling `keepalive` connections to send several requests via one connection
- Reusing SSL session parameters to avoid SSL handshakes for parallel and subsequent connections

Sessions are stored in the SSL session cache shared between worker processes and configured by the `ssl_session_cache` directive. One megabyte of cache contains about 4000 sessions. The default cache timeout is 5 minutes. This timeout can be increased using the `ssl_session_timeout` directive.

### Sample Configuration

Below is a sample configuration optimized for a multi-core system with 10 megabyte shared session cache:

```nginx
worker_processes auto;

http {
    ssl_session_cache   shared:SSL:10m;
    ssl_session_timeout 10m;

    server {
        listen              443 ssl;
        server_name         www.example.com;
        keepalive_timeout   70;

        ssl_certificate     www.example.com.crt;
        ssl_certificate_key www.example.com.key;
        ssl_protocols       TLSv1 TLSv1.1 TLSv1.2;
        ssl_ciphers         HIGH:!aNULL:!MD5;
        #...
    }
}
```

## References

[Tips for 10x performance](https://www.nginx.com/blog/10-tips-for-10x-application-performance/)
[NGINX SSL Termination](https://docs.nginx.com/nginx/admin-guide/security-controls/terminating-ssl-http/)
