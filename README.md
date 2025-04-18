# Alt Corp management app for Alliance Auth

[![Badge: Version]][pypi]
[![Badge: License]][license]
[![Badge: Supported Python Versions]][pypi]
[![Badge: Supported Django Versions]][pypi]
![Badge: pre-commit]
[![Badge: pre-commit.ci status]][pre-commit.ci status]
[![Badge: Code Style: black]][black code formatter documentation]
[![Badge: Automated Tests]][automated tests on github]
[![Badge: Code Coverage]][codecov]

## Features

Players can request an alt corp be added to your alt alliance. Requests cannot be completed without all characters in that corp having tokens in auth.

## Installing

Altcorp is a plugin for [Alliance Auth](https://gitlab.com/allianceauth/allianceauth). If you don't have Alliance Auth running already, please install it first before proceeding. (see the official [AA installation guide](https://allianceauth.readthedocs.io/en/latest/installation/auth/allianceauth/) for details)

```bash
pip install aa-altcorp
```

Configure your Auth settings (`local.py`) as follows:

- Add `'altcorp'` to `INSTALLED_APPS`
- Add the following required lines to your settings file:

```python
AC_ALT_ALLIANCE = 123456  # alt alliance ID
CELERYBEAT_SCHEDULE["altcorp_update"] = {
    "task": "altcorp.update_corp_requests_for_alliance(",
    "schedule": crontab(minute=0, hour=3),
    "apply_offset": True,
}
```

Run migrations & copy static files

```bash
python manage.py migrate
python manage.py collectstatic
```

Restart your supervisor services for Auth

## Settings

Here is a list of available settings for this app. They can be configured by adding them to your Auth settings file (`local.py`).

Note that all settings are optional and the app will use the documented default settings if they are not used.

| Name                | Description                                                                                                                                                                                | Default                      |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------- |
| `AC_ALT_ALLIANCE`   | The ID of the alt alliance.                                                                                                                                                                | `123456`                     |
| `AC_ALLIANCE_IDS`   | A list of alliance ID's. Toons in an alt corp must have their users main character as part of one of theses alliances. Blank ignores this check and will allow anyone to be in an alt corp | `[]`                         |
| `AC_WEBHOOK`        | Webhook for sending manager notifications\`                                                                                                                                                | \`\`                         |
| `AC_REVOKE_DAYS`    | Days of non compliance before the app tells you to remove a corp from alt alliance                                                                                                         | `7`                          |
| `AC_IGNORE_CORPS`   | A list of corps in the alt alliance to ignore. Use to ignore executor or other corps that don't require checks.                                                                            | `[]`                         |
| `AC_REQUIRED_SCOPE` | A list of states and scopes to check toons for.                                                                                                                                            | `{"Member": ["publicData"]}` |

## Permissions

Here are all relevant permissions:

| Codename                                 | Description                                                                     |
| ---------------------------------------- | ------------------------------------------------------------------------------- |
| `general - Can access this app`          | Basic permission required by anyone to access this app and request an alt corp. |
| `general - Can manage alt corp requests` | Users with this permission can manage requests.                                 |

[automated tests on github]: https://github.com/astrum-mechanica/aa-altcorp/actions/workflows/automated-checks.yml
[badge: automated tests]: https://github.com/astrum-mechanica/aa-altcorp/actions/workflows/automated-checks.yml/badge.svg "Automated Tests"
[badge: code coverage]: https://codecov.io/gh/astrum-mechanica/aa-altcorp/branch/master/graph/badge.svg "Code Coverage"
[badge: code style: black]: https://img.shields.io/badge/code%20style-black-000000.svg "Code Style: black"
[badge: license]: https://img.shields.io/github/license/astrum-mechanica/aa-altcorp "License"
[badge: pre-commit]: https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit&logoColor=white "pre-commit"
[badge: pre-commit.ci status]: https://results.pre-commit.ci/badge/github/astrum-mechanica/aa-altcorp/master.svg "pre-commit.ci status"
[badge: supported django versions]: https://img.shields.io/pypi/djversions/aa-altcorp?label=django "Supported Django Versions"
[badge: supported python versions]: https://img.shields.io/pypi/pyversions/aa-altcorp "Supported Python Versions"
[badge: version]: https://img.shields.io/pypi/v/aa-altcorp?label=release "Version"
[black code formatter documentation]: http://black.readthedocs.io/en/latest/
[codecov]: https://codecov.io/gh/astrum-mechanica/aa-altcorp
[license]: https://github.com/astrum-mechanica/aa-altcorp/blob/master/LICENSE
[pre-commit.ci status]: https://results.pre-commit.ci/latest/github/astrum-mechanica/aa-altcorp/master "pre-commit.ci"
[pypi]: https://pypi.org/project/aa-altcorp/
