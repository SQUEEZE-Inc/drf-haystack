Haystack for Django REST Framework
==================================

SQUEEZE compatibility build (SB-2691)
-----------------------------------

The `1.9.1+squeeze.1` distribution uses the package code from upstream
[`v1.9.1`](https://github.com/django-commons/drf-haystack/tree/f5d65ecdb04c9ada63a5a90cf758321a1aa81ae6).
Only distribution metadata widens the dependency bounds for suitebook's Django
5.2 migration: Django `>=4.2,<6.0`, DRF `>=3.12,<3.18`, and django-haystack
`>=2.8,<3.5`. Runtime `drf_haystack.__version__` intentionally remains `1.9.1`.
The wider bounds permit resolution and do not certify every allowed combination.

Build a wheel from a reviewed, immutable commit with Python 3.11 and suitebook's
build tooling (`setuptools==84.0.0`, `wheel==0.48.0`):

```bash
SOURCE_DATE_EPOCH=1731283200 python -m pip wheel --no-deps --no-build-isolation . --wheel-dir /tmp/drf-haystack-wheel
```

Validate package-file identity against the upstream 1.9.1 PyPI wheel, then run
suitebook's reservation/customer/thread/MCP search tests under both the existing
and target framework versions. Live OpenSearch acceptance is required before
the framework migration is released. This fork is the source of truth for the
temporary compatibility build; suitebook may vendor the built wheel for installs
that do not fetch Git source. Track removal/replacement in
[SB-2691](https://linear.app/squeeze/issue/SB-2691).

Build status
------------

[![Build Status](https://travis-ci.org/rhblind/drf-haystack.svg?branch=master)](https://travis-ci.org/rhblind/drf-haystack)
[![Coverage Status](https://coveralls.io/repos/github/rhblind/drf-haystack/badge.svg?branch=master)](https://coveralls.io/github/rhblind/drf-haystack?branch=master)
[![PyPI version](https://badge.fury.io/py/drf-haystack.svg)](https://badge.fury.io/py/drf-haystack)
[![Documentation Status](https://readthedocs.org/projects/drf-haystack/badge/?version=latest)](http://drf-haystack.readthedocs.io/en/latest/?badge=latest)


About
-----

Small library which tries to simplify integration of Haystack with Django REST Framework.
Fresh [documentation available](https://drf-haystack.readthedocs.io/en/latest/) on Read the docs!

Dependency ranges
------------------

- Python 3.7 and above
- Django >=4.2,<6.0
- Haystack >=2.8,<3.5
- Django REST Framework >=3.12,<3.18
- elasticsearch >=2.0.0,<=8.3.3,


Installation
------------

    $ pip install drf-haystack

Supported features
------------------
We aim to support most features Haystack does (or at least those which can be used in a REST API).
Currently, we support:

- Autocomplete
- Boost (Experimental)
- Faceting
- Geo Spatial Search
- Highlighting
- More Like This

Show me more!
-------------

```python
from drf_haystack.serializers import HaystackSerializer
from drf_haystack.viewsets import HaystackViewSet

from myapp.search_indexes import PersonIndex  # You would define this Index normally as per Haystack's documentation

# Serializer
class PersonSearchSerializer(HaystackSerializer):
    class Meta:
        index_classes = [PersonIndex]
        fields = ["firstname", "lastname", "full_name"]

# ViewSet
class PersonSearchViewSet(HaystackViewSet):
    index_models = [Person]
    serializer_class = PersonSerializer
```

That's it, you're good to go. Hook it up to a DRF router and happy searching!
