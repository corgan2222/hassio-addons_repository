# Home Assistant Community Add-on: EMQX6 Enterprise

EMQX Enterprise v6 — the most scalable open-source MQTT broker for IoT.

## Installation

1. Click the Home Assistant My button to open the add-on on your instance.
1. Click "Install".
1. Start the "EMQX6" add-on.
1. Open the Web UI and log in with `admin` / `public`.
1. Set up authentication under **Access Control** → **Authentication**.

## Configuration

Most configuration is done via the EMQX web UI. Advanced options can be set
using environment variables:

```yaml
env_vars:
  - name: EMQX_NODE__NAME
    value: emqx@127.0.0.1
```

Only variables starting with `EMQX_` are accepted.

## Support

[Open an issue][issue] on GitHub.

## Authors & contributors

The original setup is by [Franck Nijhof][frenck].
Enterprise Edition v6 fork maintained by [Stefan Knaak][corgan2222].

[corgan2222]: https://github.com/corgan2222
[frenck]: https://github.com/frenck
[issue]: https://github.com/corgan2222/addon-emqx6/issues
