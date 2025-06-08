### Local Synapse Setup

You'll want at least two locally running Synapse instances to hack on Matrixbird locally. Here is how I have two local homeservers `one.local` and `two.local` set up. 

Add these to your `/etc/hosts` file:

```
127.0.0.1   localhost
...
127.0.0.1   one.local
127.0.0.1   two.local
```

Synapse config should look like this:

```yaml
server_name: "one.local"
pid_file: /home/ah/dev/matrixbird/homeservers/one.local/homeserver.pid
listeners:
  - port: 8008
    tls: false
    type: http
    x_forwarded: true
    bind_addresses: ['::1', '127.0.0.1']
    resources:
      - names: [client, federation]
        compress: false
public_baseurl: "https://one.local"
database:
  name: psycopg2
  args:
    user: postgres
    password: postgres
    database: one.local
    host: localhost
    cp_min: 5
    cp_max: 10
log_config: "/home/ah/dev/matrixbird/homeservers/one.local/one.local.log.config"
media_store_path: /home/ah/dev/matrixbird/homeservers/one.local/media_store
registration_shared_secret: "0Qbu.Fccxo,TcrdKRzCCt3o;fx6V#t=9fY:iQk=HS@hi.0#D1S"
report_stats: false
macaroon_secret_key: "C#sdb=ldyI;7LhMYnp7JHHE:^G^rgigS2LD#~JJpQ+tlFGPoZW"
form_secret: "~-OLsjDDXT@I.-36~HCS29mODUR4y#KueZ7Xh9o2na1W8J_~OL"
signing_key_path: "/home/ah/dev/matrixbird/homeservers/one.local/one.local.signing.key"
trusted_key_servers: []
suppress_key_server_warning: true
accept_keys_insecurely: true
enable_registration: true
enable_registration_without_verification: true
ip_range_whitelist:
  - '127.0.0.1/8'
  - '::1/128'
```

Create local certs using `mkcert` like so:

```shell
> mkcert one.local
> mkcert two.local
```

Put synapse behind nginx reverse proxy:

```nginx
server {
    listen 443 ssl http2;
    listen 8448 ssl http2;
    server_name one.local;

    ssl_certificate /home/ah/dev/matrixbird/homeservers/certs/one.local.pem;
    ssl_certificate_key /home/ah/dev/matrixbird/homeservers/certs/one.local-key.pem;

    location /.well-known/matrix/server {
        return 200 '{"m.server": "one.local:443"}';
        add_header Content-Type application/json;
        add_header Access-Control-Allow-Origin *;
    }

    location /.well-known/matrix/client {
        return 200 '{"m.homeserver": {"base_url": "https://one.local"}}';
        add_header Content-Type application/json;
        add_header Access-Control-Allow-Origin *;
    }

    location / {
        proxy_pass http://127.0.0.1:8008;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    client_max_body_size 50M;
    proxy_http_version 1.1;
    }

}
```

These instances should now be able to federate and talk to each other. Test that you can reach `https://one.local` and `https://two.local` in your browser, they should show the default matrix static page. You may need to restart your browser for the certs to work properly.

In Element, of your client of choice, create accounts on both homeservers to test if everything is set up correctly. 
