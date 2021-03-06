A simple way to use `jSignature jQuery plugin <https://github.com/brinley/jSignature/blob/master/README.md>`_ in your `Django <https://www.djangoproject.com>`_ projects.

It provides:

* A form field and a form widget to handle jquery plugin through a Django form;
* A model field to store a captured signature;
* A mixin adding two fields (signature / signature_date) in any of your Django models.

.. image:: https://travis-ci.org/fle/django-jsignature.png?branch=master
        :target: https://travis-ci.org/fle/django-jsignature

.. image:: https://coveralls.io/repos/fle/django-jsignature/badge.png
       :target: https://coveralls.io/r/fle/django-jsignature


.. image:: https://github.com/fle/django-jsignature/blob/master/screen.png

==================
INSTALL
==================

* In your terminal:

::

    pip install django-jsignature

* Add ``'jsignature'`` to your ``INSTALLED_APPS``:

.. code:: python

    # settings.py
    INSTALLED_APPS = (
        ...
        'jsignature',
    )

==================
USAGE
==================

* Use provided form field and widget:

.. code:: python

    # forms.py
    from django import forms
    from jsignature.forms import JSignatureField

    class SignatureForm(forms.Form):
        signature = JSignatureField()

* In your template:

.. code:: html

    <!-- index.html -->
    {{ form.media }}
    <form action="." method="POST">
        {% for field in form %}
            {{ field.label_tag }}
            {{ field }}
        {% endfor %}
        <input type="submit" value="Save"/>
        {% csrf_token %}
    </form>

* Render image after form validation:

.. code:: python

    # views.py
    from jsignature.utils import draw_signature
    import base64
    from io import BytesIO
    from myapp.forms import SignatureForm

    def my_view(request):
        form = SignatureForm(request.POST or None)
        if form.is_valid():
            signature = form.cleaned_data.get('signature')
            if signature:
                # as an image
                signature_draw = draw_signature(signature)
                buffer = BytesIO()
                signature_draw.save(buffer, "PNG")
                signature_picture = base64.b64encode(buffer.getvalue()).decode('ascii')
                # or as a file
                signature_file_path = draw_signature(signature, as_file=True)

==================
CUSTOMIZATION
==================

JSignature plugin options are available in python:

* Globally, in your settings:

.. code:: python

    # settings.py
    JSIGNATURE_WIDTH = 500
    JSIGNATURE_HEIGHT = 200

* Specifically, in your form:

.. code:: python

    # forms.py
    from jsignature.forms import JSignatureField
    from jsignature.widgets import JSignatureWidget

    JSignatureField(widget=JSignatureWidget(jsignature_attrs={'color': '#CCC'}))

Available settings are:

* ``JSIGNATURE_WIDTH`` (width)
* ``JSIGNATURE_HEIGHT`` (height)
* ``JSIGNATURE_COLOR`` (color)
* ``JSIGNATURE_BACKGROUND_COLOR`` (background-color)
* ``JSIGNATURE_DECOR_COLOR`` (decor-color)
* ``JSIGNATURE_LINE_WIDTH`` (lineWidth)
* ``JSIGNATURE_UNDO_BUTTON`` (UndoButton)
* ``JSIGNATURE_RESET_BUTTON`` (ResetButton)

==================
IN YOUR MODELS
==================

If you want to store signatures, provided mixin gives a ``signature`` and a ``signature_date`` that update themselves:

.. code:: python

    from django.db import models
    from jsignature.mixins import JSignatureFieldsMixin

    class JSignatureModel(JSignatureFieldsMixin):
        name = models.CharField()


==================
AUTHORS
==================

    * Florent Lebreton <florent.lebreton@makina-corpus.com>

|makinacom|_

.. |makinacom| raw:: html

    <img src="http://depot.makina-corpus.org/public/logo.gif" height="100px">

.. _makinacom: http://www.makina-corpus.com


==================
UPDATES
==================

    * Nadine Project <info@nadineproject.org>

|nadineeu|_

.. |nadineeu| raw:: html

    <img src="http://nadine-project.eu/wp-content/uploads/2019/04/NadineV3-36-1.png" height="100px">

.. _nadineeu:  http://nadine-project.eu/


    * Hicham Bakri <hicham.bakri@etu.enseeiht.fr>

|enseeihtfr|_

.. |enseeihtfr| raw:: html

    <img src="logoenseeiht.png" height="50px">

.. _enseeihtfr:  http://www.enseeiht.fr/en/index.html

