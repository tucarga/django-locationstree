=====================
Django Locations Tree
=====================

Simple application for location field as TreeForeignKey (see django-mptt) with form field.

Install
=======

::

  pip install django-locationstree

  python manage.py migrate

Usage
=====

::

  # settings.py
  INSTALLED_APPS += ('mptt', 'locationstree')

  # models.py
  class Place(models.Model):
    location = TreeForeignKey('locationstree.Location')

  # forms.py
  class PlaceForm(models.Model):
    location = JustLeafsLocationField(queryset=Location.objects.all())


Context Processor
=================

There is a context processor that sets Locations.objects.all() to context with the variable name by default `locations`::

  # settings.py
  CONTEXT_PROCESSORS += ('locationstree.context_processors.locations',)


The variable name is configurable via settings variable::

  # settings.py
  LOCATIONSTREE_CONTEXT_VARIABLE_NAME = 'lugares_comunes'

It uses cache with the key name `locationstree.locations` and a TTL of 1 week configurable via settings variable::

  # settings.py
  LOCATIONSTREE_CACHE_TTL = 60 * 60 * 24 # 1 day

Fixtures
========
Call `python manage.py loaddata <fixture_name>` to load the fixture
into your database.

Chile
-----
`chile.json`: Defines all regions and its corresponding comunes

Levels
======

Defines a name for each `level` defined in a fixture file in case you
want to do something like filtering:

.. code:: python

    class MyModel(models.Model):
        coverage_region = TreeForeignKey(
            'locationstree.Location',
            limit_choices_to={'level': locationstree.levels.CHILE['region']}
            )

This just make easier to filter `level` by "region" to limit choices.
