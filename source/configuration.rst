Oblog Config Annotations
========================

Oblog supports data-binding of code values through the ``@Config`` annotation and a variety of widget-specific sub-annotations.

Using the Config Annotations
----------------------------

The @Config Annotation
^^^^^^^^^^^^^^^^^^^^^^

.. note:: The Shuffleboard values displayed for data-binding via setter methods are one-way; changes to the value in code will *not* be reflected on the dashboard.  This holds true even if the value is modified by Oblog itself; if two data-binding widgets are configured for the same setter, their displayed values will not automatically remain synchronized.  The value in code will correspond to whichever widget was most-recently updated.

The ``@Config`` annotation may be used to perform data-binding on boolean- or numeric-valued setters and ``Sendable`` fields, as follows:

.. code:: java

  @Config
  void setExampleValue(double value) {
    exampleValue = value;
  }

.. code:: java

  @Config
  PIDController exampleController;

Like all Oblog annotations, this works regardless of access modifiers (e.g. ``public`` and ``private``).

The ``@Config`` annotation is the most-general annotation available in Oblog.  It will use a default widget type inferred from the parameter type of the setter or the type of the field, and will work with any of Oblog's supported data types.

If an alternative widget is preferred - or if you wish to configure the logging further with widget-specific parameters - one of the widget-specific sub-annotations can be used.  For a list of these, see `the API docs <https://oblarg.github.io/Oblog/io/github/oblarg/oblog/annotations/package-summary.html>`__.

Multi-Parameter Setters
^^^^^^^^^^^^^^^^^^^^^^^

.. warning:: If individual parameter names are not specified with parameter-level annotations, Oblog will attempt to infer them from the parameter names in code.  This functionality requires the optional ``-parameters`` java compiler flag - be sure to add this to your ``build.gradle`` if you wish to use this functionality.  If this is not set, and individual names are not provided, the widgets will be titled ``arg0``, ``arg1``, ``arg2``, etc.

The ``@Config`` annotation also works with multi-parameter setters:

.. code::

  @Config
  void setPID(double p, double i, double d) {
    controller.setPID(p, i, d);
  {

If a multi-parameter setter is annotated, it will display on the dashboard as a layout, with the individual parameters as widgets in the layout.  The widgets of the individual parameters can be controlled as one normally would a single-parameter setter by annotating each of the method parameters individually with ``@Config``.

Sendable Widgets
^^^^^^^^^^^^^^^^

Many Shuffleboard widget types correspond to objects that implement the WPILib ``Sendable`` interface.  To configure these types of data, *only* field annotations should be used, rather than setter annotations, as Oblog will assume that they are persistent objects.

Parameters
----------

The various configurating annotations take allow configuration of the bound value through a number of parameters.  This section describes those parameters that are common to all of the configuration annotations; for a detailed description of the widget-specific parameters, see `the API docs <https://oblarg.github.io/Oblog/io/github/oblarg/oblog/annotations/package-summary.html>`__.

Value Name
^^^^^^^^^^

The name of the value on shuffleboard (and of its corresponding NetworkTables key) can be set with the ``name`` parameter.

If the name parameter is not provided, Oblog will use the name of the annotated setter or field.

Tab Name
^^^^^^^^

The ``tabName`` parameter can be used to explicitly override Oblog's inferred tab structure.  If the ``tabName`` parameter is set, the bound value will be displayed under a Shuffleboard tab of the specified name.

Method Name/Types
^^^^^^^^^^^^^^^^^

The ``methodName`` and ``methodTypes`` parameters can be used to extract a setter from a complex type for data-binding.  To do this, use the parameters to specify the name and argument types of the setter.  For example:

.. code:: java

  public class HasSetter {
    public void setFoo(double foo) {
      // what is done with the argument is unimportant
    }
  }

.. code:: java

  @Config(methodName = "setFoo", methodTypes = {double.class})
  HasSetter hasSetter;

Default Values
^^^^^^^^^^^^^^

When binding data through a setter, it's important to provide a default value for the bound variable.  This can be specified with the ``defaultValueNumeric`` or ``defaultValueBoolean`` parameter.  When the logger runs its initial configuration method, the default value will be passed to the setter.  If a default value is not provided by the user, Oblog will use ``0`` for numeric data types or ``false`` for boolean data types.

Multi-Parameter Layout Type
^^^^^^^^^^^^^^^^^^^^^^^^^^^

When performing :ref:`data-binding on multi-parameter setters <configuration:Multi-Parameter Setters>`, users have a choice of using either a list layout or a grid layout.  This can be set with the ``multiArgLayoutType`` parameter, which can take values of either ``"listLayout"`` or ``"gridLayout"``.  If this is not specified, it will default to a list layout.

If a grid layout is used, the number of rows/columns in the internal grid can be set with the ``numGridRows`` and ``numGridColumns`` parameters.  Both will default to a value of ``3`` if not provided.

Display Size
^^^^^^^^^^^^

The size of the displayed Shuffleboard widget can be set with the ``width`` and ``height`` parameters.  If none are provided, the default Shuffleboard size for the widget type is used.  Size is measured in Shuffleboard grid units.

Display Location
^^^^^^^^^^^^^^^^

.. warning:: If the position of a single widget or layout on a tab is manually specified, the position of *all* widgets and layouts on the tab should be manually specified.  Failure to do so will likely result in overlapping/hidden widgets, since Shuffleboard's auto-placement algorithm does not interact nicely with manually-positioned widgets.

The location of the displayed widget in its tab can be set with the ``rowIndex`` and ``columnIndex`` parameters.  The position is measured in Shuffleboard grid units.  If position is not set, Shuffleboard will automatically place the widget.
