# tls_private_key

Specify the path to the TLS private key file that corresponds to the `tls_certificate`.

The default, if unspecified, is to dynamically allocate a self-signed certificate.

{{since('2025.05.06-b29689af', indent=True)}}
    The private key will be cached for 5 minutes, then re-evaluated,
    allowing for the privae key to be updated without restarting
    the service. In prior versions of KumoMTA you would need to
    restart kumod in order to pick up an updated private key.


```lua
kumo.start_esmtp_listener {
  -- ..
  tls_private_key = '/path/to/key.pem',
}
```

You may specify that the key be loaded from a [HashiCorp Vault](https://www.hashicorp.com/products/vault):

```lua
kumo.start_esmtp_listener {
  -- ..
  tls_private_key = {
    vault_mount = 'secret',
    vault_path = 'tls/mail.example.com.key',
    -- Optional: specify a custom key name (defaults to "key")
    -- vault_key = "private_key"

    -- Specify how to reach the vault; if you omit these,
    -- values will be read from $VAULT_ADDR and $VAULT_TOKEN

    -- vault_address = "http://127.0.0.1:8200"
    -- vault_token = "hvs.TOKENTOKENTOKEN"
  },
}
```

The key must be stored under the `path` specified. By default, it looks for a field named `key` in the vault secret.
For example, you might populate it like this:

```
$ vault kv put -mount=secret tls/mail.example.com key=@mail.example.com.key
```

If you want to use a different field name, you can specify it with `vault_key` {{since('dev', inline=True)}}:

```lua
kumo.start_esmtp_listener {
  -- ..
  tls_private_key = {
    vault_mount = 'secret',
    vault_path = 'tls/mail.example.com.key',
    vault_key = 'private_key',  -- Look for 'private_key' instead of 'key'
  },
}
```

And store it in vault like this:

```
$ vault kv put -mount=secret tls/mail.example.com private_key=@mail.example.com.key
```


