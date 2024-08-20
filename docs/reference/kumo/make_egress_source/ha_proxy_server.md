# ha_proxy_server

Optional string.

If both `ha_proxy_server` and `ha_proxy_source_address` are specified, then
SMTP connections will be made via an HA Proxy server.

`ha_proxy_server` specifies the address and port of the proxy server.

```lua
kumo.on('get_egress_source', function(source_name)
  if source_name == 'ip-1' then
    -- Make a source that will emit from 10.0.0.1, via a proxy server
    kumo.make_egress_source {
      name = 'ip-1',
      ha_proxy_source_address = '10.0.0.1',
      ha_proxy_server = '10.0.0.1:5000',
      ehlo_domain = 'mta1.examplecorp.com',
    }
  end
  error 'you need to do something for other source names'
end)
```

