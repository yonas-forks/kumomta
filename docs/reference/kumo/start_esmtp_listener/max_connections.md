# max_connections

{{since('dev')}}

Specifies the maximum number of concurrent connections that are permitted
to this listener. Connections above this will be accepted and then closed
immediately with a `421 4.3.2` response.

The default value for this is `32768`, which is approximately half of
the possible number of ports for a given IP address, leaving the other
half available for outgoing connections.

Each time a connection is denied due to hitting this limit, the
`total_connections_denied` counter is incremented for the `esmtp_listener`
service.

In earlier releases, there was no kumod-controlled upper bound on the
number of connections, and as many as the kernel allowed would be
permitted.

