Django Social Share
======================================

.. image:: https://travis-ci.org/fcurella/django-social-share.svg?branch=master
    :target: https://travis-ci.org/fcurella/django-social-share

.. image:: https://coveralls.io/repos/github/fcurella/django-social-share/badge.svg?branch=master
    :target: https://coveralls.io/github/fcurella/django-social-share?branch=master

Provides tempatetags for 'Tweet This', 'Share this on Facebook', 'Share on Google+' and 'mailto://'.

Installation
-------------

::

    $ pip install django-social-share

Add the app to ``INSTALLED_APPS``::

  INSTALLED_APPS += ['django_social_share']

It's recommended to add ``django.core.context_processors.request`` to your `` TEMPLATE_CONTEXT_PROCESSORS`` list. This way the templatetags will use the correct scheme and hostname.

If ``django.core.context_processors.request`` is not present, it will simply concatenate the current site's domain (from ``django.contrib.sites``) and the object's relative URL together.

Usage
-----
::

  {% post_to_facebook <object_or_url> <link_text> %}
  
  {% post_to_gplus <object_or_url> <link_text> %}

  {% post_to_twitter <text_to_post> <object_or_url> <link_text> %}
  
  {% post_to_mail <email_subject> <text_to_post> <object_or_url> <link_text> %}

  {% send_email <subject> <text_to_post> <object_or_url> <link_text> %}

``<text_to_post>`` may contain any valid Django Template code. Note that Facebook does not support this anymore.

``<object_or_url>`` is optional. If you pass a django model instance, it will use its ``get_absolute_url`` method. Additionally, if you have ``django_bitly`` installed, it will use its shortUrl on Twitter.

``<link_text>`` is also optional. It defines the text used for the ``a`` element. Defaults to 'Post to Facebook' and 'Post to Twitter'.

``<subject>`` may contain any valid Django Template code.

::

  {% post_to_twitter_url <text_to_post> <object_or_url> %}

Will add a ``tweet_url`` variable to the context, containing the URL for the Twitter sharer popup.

::

  {% post_to_facebook_url <object_or_url> %}

Will add a ``facebook_url`` variable to the context, containing the URL for the Facebook sharer popup.

::

  {% post_to_gplus_url <object_or_url> %}

Will add a ``gplus_url`` variable to the context, containing the URL for the Google+ sharer popup.

::

  {% send_email_url <subject> <text_to_post> <object_or_url> <link_text> %}

Will add a ``mailto_url`` variable to the context, containing the URL for the ``mailto``anchor.

Example::

  {% load social_share %}
  
  {% post_to_facebook object_or_url "Post to Facebook!" %}
  {% post_to_twitter "New Song: {{object.title}}. Check it out!" object_or_url "Post to Twitter" %}
  {% post_to_gplus object_or_url "Post to Google+!" %}
  {% send_email object.title "New Song: {{object.title}}. Check it out!" object_or_url "Share via email" %}

Templates are in ``django_social_share/templatetags/post_to_twitter.html``, ``django_social_share/templatetags/post_to_facebook.html`` and ``django_social_share/templatetags/post_to_gplus.html``, ``django_social_share/templatetags/send_email.html``. You can override them to suit your mileage.
