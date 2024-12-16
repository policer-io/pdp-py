[![logo-light-long-2](https://github.com/policer-io/.github/assets/16650977/c39ad4a3-7a5c-40b6-9a69-5be3a3c50255)](https://policer.io)

# Policy Decision Point — Python

The [policer.io](https://policer.io) Policy Decision Point (PDP) client library for Python projects.

[![Pipeline](https://github.com/policer-io/pdp-py/actions/workflows/test.yml/badge.svg)](https://github.com/policer-io/pdp-py/actions/workflows/test.yml)
[![PyPI](https://img.shields.io/static/v1?label=shipped+with&message=PyPI&color=0073b7)](https://pypi.org/project/policer-io-pdp)
[![embrio.tech](https://img.shields.io/static/v1?label=🚀+by&message=EMBRIO.tech&color=24ae5f)](https://embrio.tech)

## :star: Give us a Star!

### Support the project by giving it a GitHub Star!

[![GitHub Repo stars](https://img.shields.io/github/stars/policer-io/pdp-py?label=GitHub%20%E2%AD%90%EF%B8%8F)](https://github.com/policer-io/pdp-py)

## :gem: Why `policer-io-pdp`?

Advanced access control with one line of code with policy as data:

```python
from policer-io-pdp import PDP

async def main():
    pdp = await PDP.create(
        application_id="65f0674f39d8a1a5ef805ca7",
        hostname="cloud.policer.io"
    )

    decision = pdp.can(
        role=["editor", "publisher"],
        operation="article:publish",
        attributes={
            "user": {"_id": "some-user-id-003"},
            "document": {
                "published": False,
                "createdBy": "other-user-id-007"
            }
        }
    )

    if decision.grant:
        # Authorized
        print("Access granted!")

        # use decision.filter, decision.projection, and decision.setter
    else:
        # Not authorized
        raise Exception("403 Forbidden")
```

[Learn more](https://policer.io/#features) about the benefits and features of policer.io!

## :floppy_disk: Installation

:construction: Work in Progress. Not yet published. Ready Soon!

### Prerequisites
TODO: check python version

- Pyton >= v3.x is required
- policer.io Policy Center instance ([learn more](https://policer.io/#about))
  - self-hosted
  - https://cloud.policer.io (coming soon)

### Install

Use yarn command

    pip install policer-io-pdp

## :orange_book: Usage

### Connect to Policy Center

The PDP connects to a policer.io Center Instance to load the policy (roles and permissions) for a given application. Therefore create and connect a `PDP` instance with:

```python
from policer-io-pdp import PDP

type RoleName = 'reader' | 'editor' | 'publisher'

pdp = await PDP.create(
    application_id="65f0674f39d8a1a5ef805ca7",
    hostname="cloud.policer.io"
)
```

### Make Policy Decisions

```python
# 1. prepare policy decision inputs

# the user's roles
roles = ["editor", "publisher"]
# the operation to check
operation = "article:publishBatch"
# attributes of user, document or context
attributes = {
    "user": {
        "_id": "some-user-id-003",
        "magazine": "The New Yorker"
    },
    "document": {
        "published": False,
        "createdBy": "other-user-id-007"
    }
}

# 2. perform policy decision/check
decision = pdp.can(roles, operation, attributes)


# 3. use policy decision result
if decision.grant:
    # if authorized

    # query documents and document properties based on policy decision result (`filter` & `projection`)
    articles = db.articles.find(
        {"$and": [{"status": "ready"}, decision.filter]},
        decision.projection
    ).exec()

    # set or overwrite some document fields based on policy decision result (`setter`)
    # for example `article.magazine`
    for article in articles:
        article_dict = {**article, **decision.setter}  # merge article with setter
        publish(article_dict)
else:
    # if not authorized
    raise HTTPException(status_code=403, detail="Forbidden")
```

## :bug: Bugs

Please report bugs by creating a [bug issue](https://github.com/policer-io/pdp-py/issues/new?assignees=&labels=Bug&template=bug.md&title=).

## :construction_worker_man: Contribute

You can contribute to policer.io by

- improving typescript PDP (this package)
- implementing policer.io PDP for other programming languages
- developing on the policer.io ecosystem in general

Either way, [let's talk](https://policer.io/contact/)!

### Development

#### Prerequisites

:construction: TODO

- Python 

#### Install

    python ...

#### Test

    python ...

or

    python ...:watch

#### Commit

This repository uses commitlint to enforce commit message conventions. You have to specify the type of the commit in your commit message. Use one of the [supported types](https://github.com/pvdlg/conventional-changelog-metahub#commit-types).

    git commit -m "[type]: my perfect commit message"

## :speech_balloon: Contact

Talk to us via [policer.io](https://policer.io/contact/)

## :lock_with_ink_pen: License

The code is licensed under the [MIT License](https://github.com/policer-io/pdp-py/blob/main/LICENSE)
