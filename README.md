# Home Assistant Community App by corgan2222

![Project Stage][project-stage-shield]
![Maintenance][maintenance-shield]
[![License][license-shield]](LICENSE.md)

## About

A personal Home Assistant add-on repository based on the
[Home Assistant Community Apps](https://github.com/hassio-addons/repository) project.

## Installation

Add this repository to your Home Assistant instance via the add-on store:

[![Add repository to Home Assistant][repo-badge]][repo-url]

Or manually add this URL:

```txt
https://github.com/corgan2222/hassio-addons_repository
```

## Apps provided by this repository

### &#10003; [EMQX Enterprise][addon-emqx]

![Addon Version][emqx-version-shield]
![EMQX Version][emqx-app-version-shield]
![Supports aarch64 Architecture][emqx-aarch64-shield]
![Supports amd64 Architecture][emqx-amd64-shield]

The most scalable open-source MQTT broker for IoT. Enterprise Edition — an alternative
to the Mosquitto add-on with Flow Designer, Rule Engine, and advanced access control.

[:books: EMQX Enterprise app documentation][addon-doc-emqx]

### &#10003; [Grafana][addon-grafana]

![Latest Version][grafana-version-shield]
![Supports aarch64 Architecture][grafana-aarch64-shield]
![Supports amd64 Architecture][grafana-amd64-shield]

The open platform for beautiful analytics and monitoring

[:books: Grafana app documentation][addon-doc-grafana]

## Support

Open an issue in the relevant repository:

- [Open an issue for the app: EMQX Enterprise][emqx-issue]
- [Open an issue for the app: Grafana][grafana-issue]

For a general repository issue [open an issue here][issue]

## License

MIT License

Copyright (c) 2017-2026 Franck Nijhof

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

[repo-badge]: https://my.home-assistant.io/badges/supervisor_add_addon_repository.svg
[repo-url]: https://my.home-assistant.io/redirect/supervisor_add_addon_repository/?repository_url=https%3A%2F%2Fgithub.com%2Fcorgan2222%2Fhassio-addons_repository

[addon-emqx]: https://github.com/corgan2222/addon-emqx/tree/v0.8.3
[addon-doc-emqx]: https://github.com/corgan2222/addon-emqx/blob/main/README.md
[emqx-issue]: https://github.com/corgan2222/addon-emqx/issues
[emqx-version-shield]: https://img.shields.io/github/v/release/corgan2222/addon-emqx?label=addon
[emqx-app-version-shield]: https://img.shields.io/badge/EMQX-e5.10.3-brightgreen.svg
[emqx-aarch64-shield]: https://img.shields.io/badge/aarch64-yes-green.svg
[emqx-amd64-shield]: https://img.shields.io/badge/amd64-yes-green.svg

[addon-grafana]: https://github.com/corgan2222/addon-grafana/tree/v12.4.3.1
[addon-doc-grafana]: https://github.com/corgan2222/addon-grafana/blob/v12.4.3.1/README.md
[grafana-issue]: https://github.com/corgan2222/addon-grafana/issues
[grafana-version-shield]: https://img.shields.io/badge/version-v12.4.3.1-blue.svg
[grafana-aarch64-shield]: https://img.shields.io/badge/aarch64-yes-green.svg
[grafana-amd64-shield]: https://img.shields.io/badge/amd64-yes-green.svg

[issue]: https://github.com/corgan2222/hassio-addons_repository/issues
[license-shield]: https://img.shields.io/github/license/corgan2222/hassio-addons_repository.svg
[maintenance-shield]: https://img.shields.io/maintenance/yes/2026.svg
[project-stage-shield]: https://img.shields.io/badge/project%20stage-experimental-yellow.svg
