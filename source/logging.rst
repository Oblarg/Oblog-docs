Oblog Log Annotations
=====================

Oblog supports logging of telemetry data through the ``@Log`` annotation and a variety of widget-specific sub-annotations.

Using the Log Annotations
-------------------------

The @Log Annotation
^^^^^^^^^^^^^^^^^^^

The ``@Log`` annotation can be used on both fields and getters, as follows:

.. code:: java

  @Log
  int exampleField;

.. code:: java

  @Log
  int getExampleValue() {
    return exampleValue;
  }

Like all Oblog annotations, this works regardless of access modifiers (e.g. ``public`` and ``private``).

The ``@Log`` annotation is the most-general annotation available in Oblog.  It will use a default widget type inferred from the type of the field or the return type of the getter, and will work with any of Oblog's supported data types.

If an alternative widget is preferred - or if you wish to configure the logging further with widget-specific parameters - one of the widget-specific sub-annotations can be used.  For a list of these, see `the API docs <https://oblarg.github.io/Oblog/io/github/oblarg/oblog/annotations/package-summary.html>`__.

Sendable Widgets
^^^^^^^^^^^^^^^^

Many Shuffleboard widget types correspond to objects that implement the WPILib ``Sendable`` interface.  To log these types of data, *only* field annotations should be used, rather than getter annotations, as Oblog will assume that they are persistent objects.

Parameters
----------

The various logging annotations take allow configuration of the logged value through a number of parameters.  This section describes those parameters that are common to all of the logging annotations; for a detailed description of the widget-specific parameters, see `the API docs <https://oblarg.github.io/Oblog/io/github/oblarg/oblog/annotations/package-summary.html>`__.

Value Name
^^^^^^^^^^

The name of the value on shuffleboard (and of its corresponding NetworkTables key) can be set with the ``name`` parameter.

If the name parameter is not provided, Oblog will use the name of the annotated field or getter.

Tab Name
^^^^^^^^

The ``tabName`` parameter can be used to explicitly override Oblog's inferred tab structure.  If the ``tabName`` parameter is set, the logged value will be displayed under a Shuffleboard tab of the specified name.

Method Name
^^^^^^^^^^^

Sometimes it is desirable to extract a value of a supported data type from a field of a complex data type.  This can be done using the ``methodName`` parameter.  If the ``methodName`` parameter is set, rather than the field itself being logged, the result of calling the named method on the field will be logged, instead.  The ``methodName`` functionality only works with field annotations.

Display Size
^^^^^^^^^^^^

The size of the displayed Shuffleboard widget can be set with the ``width`` and ``height`` parameters.  If none are provided, the default Shuffleboard size for the widget type is used.  Size is measured in Shuffleboard grid units.

Display Location
^^^^^^^^^^^^^^^^

.. warning:: If the position of a single widget or layout on a tab is manually specified, the position of *all* widgets and layouts on the tab should be manually specified.  Failure to do so will likely result in overlapping/hidden widgets, since Shuffleboard's auto-placement algorithm does not interact nicely with manually-positioned widgets.

The location of the displayed widget in its tab can be set with the ``rowIndex`` and ``columnIndex`` parameters.  The position is measured in Shuffleboard grid units.  If position is not set, Shuffleboard will automatically place the widget.
