Django tips and tricks
======================

Making sure you have a minimal context with a custom ``render``
---------------------------------------------------------------

Depending on the configuration of your apps, and even projects, you may realize that you always need a minimal context to set and display some elements that will always be the same on every page.

We will show here how to acheive this.

.. note::

   This method will use `class-based views <https://docs.djangoproject.com/en/3.0/topics/class-based-views/>`_. If you want to do the same will more classic views (function-based), it's up to you to adapt the method proposed here.

The idea here is to make a new class ``MyCustomView`` that will inherit from ``django.views.View``. In this class we will define two methods ``get_context`` and ``render``. We will then make all our class-based view inherit from our ``MyCustomView``.

The ``get_context`` method
^^^^^^^^^^^^^^^^^^^^^^^^^^

This method will be in charge of gathering all the required values for a properly working minimal context:

.. code-block::

    def get_context(self):
        context = {
           # All the values you need to have
        }
        return context

.. note::

    If in a view that inherits from ``MyCustomView``, you decide to override the method, be sure to call the overriden method inside the new, or to gather all the values required, otherwise you may encounter some problems.

The custom ``render`` method
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The idea here is to define a ``render`` that will make sure there's a minimal context and that all required values are there. After checking that, it calls ``django.shortcuts.render`` to render the called view.

.. code-block::

    def render(self, request, template_name, context=None, content_type=None, status=None, using=None):
        # We make sure that a context is passed
        if not context:
            context = self.get_context()
        else:
            # A context is passed, we then need to make
            # sure that all the required values are there
            general_context = self.get_context()
            for key, value in general_context.items():
                if not context.get(key):
                    context[key] = value
        return render(request, template_name, context=context, content_type=content_type, status=status, using=using)

.. warning::

    The function does not check that the required values are in the context, with the right value. If you decide in you view to change the value of a required parameter, it's your problem to make sure everything will work.

.. note::

    If you decide to override this method, be sure to call the overriden method or to check yourself that all required values are here.

The complete ``MyCustomView`` with a working example
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

File ``my_custom_view.py``

.. code-block::

    from django.views import View
    from django.shortcuts import render

    class MyCustomView(View):

        def get_context(self):
            context = {
                # All the values you need to have
            }
            return context

        def render(self, request, template_name, context=None, content_type=None, status=None, using=None):
            # We make sure that a context is passed
            if not context:
                context = self.get_context()
            else:
                # A context is passed, we then need to make
                # sure that all the required values are there
                general_context = self.get_context()
                for key, value in general_context.items():
                    if not context.get(key):
                        context[key] = value
            return render(request, template_name, context=context, content_type=content_type, status=status, using=using)

File ``views.py``

.. code-block::

    from my_custom_view import MyCustomView

    class MyView(MyCustomView):

        def get(self, request):
            # I do nothing on this view, so I don't need to call
            # self.get_context(), self.render() will do it for us.

            return self.render(request, 'my_app/template_for_my_view.html')

    class MyOtherView(MyCustomView):

        def get(self, request):
            # I do some stuff on this view, so I can either call
            # self.get_context() and add my values, or pass my context to
            # self.render() who will add the missing required values

            context = self.get_context()
            # Here I compute some stuff

            return self.render(request, 'my_app/template_for_my_view.html', context=context)
