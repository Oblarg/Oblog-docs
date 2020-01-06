Creating Loggable Classes
=========================

One of Oblog's most powerful features is the ability to infer a tab/layout structure from the structure of the user's code, removing the need for the user to worry about specifying the location of their logged widgets on the dashboard.

In order to do this, users must use the `Loggable <https://oblarg.github.io/Oblog/io/github/oblarg/oblog/Loggable.html>`__ interface.

Using the Loggable Interface
----------------------------

Making a Class Loggable
^^^^^^^^^^^^^^^^^^^^^^^

All of the methods in the ``Loggable`` interface are defaulted.  Thus, marking a class as loggable, in the simplest case, requires nothing more than declaring that it implements the ``Loggable`` interface:

.. code:: java

  public class Foo implements Loggable {
    // your class here
  }

Once a class has been declared ``Loggable``, Oblog will automatically search it for annotated fields, getters, and setters to populate the dashboard - as long as it is reachable from the specified `root container <https://oblarg.github.io/Oblog/io/github/oblarg/oblog/Logger.html#configureLoggingAndConfig(java.lang.Object,boolean)>`__ through a direct sequence of ``Loggable`` parent classes (the logger will recursively search down the tree of all ``Loggable`` fields from the root container).

``Loggable`` classes located directly in the root container will be given their own tabs.  ``Loggable`` classes located *inside of other loggable classes* will be given layouts in the tabs (or layouts) corresponding to their parents.

Configuring Loggable Names
^^^^^^^^^^^^^^^^^^^^^^^^^^

The name of the ``Loggable`` class's tab or layout can be configured by overriding the ``configureLogName`` method to return the desired name.  By default, the simple class name is used.

To avoid namespace collisions when many instances of the same ``Loggable`` class are present, it is highly recommended to override this method to return a name that is indexed based on a constructor parameter (such as a port number).

Configuring Loggable Layouts
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

When a ``Loggable`` class is located inside of another ``Loggable`` class, it is displayed as a layout.  Shuffleboard contains two types of layouts - list layouts and grid layouts - and each layout type has a number of configuration options.  A number of ``Loggable`` methods can be overridden to specify layout configuration.

Layout Type
~~~~~~~~~~~

Layout type can be configured by overriding the ``configureLayoutType()`` method to return the desired layout type.  A list layout is used by default.

Layout Size
~~~~~~~~~~~

Layout size can be configured by overriding the ``configureLayoutSize()`` method to return a two-element integer array corresponding to the desired layout width and height.  If left defaulted, Shuffleboard will automatically determine layout size.

Layout Position
~~~~~~~~~~~~~~~

.. warning:: If the position of a single widget or layout on a tab is manually specified, the position of *all* widgets and layouts on the tab should be manually specified.  Failure to do so will likely result in overlapping/hidden widgets, since Shuffleboard's auto-placement algorithm does not interact nicely with manually-positioned widgets.

Layout position can be configured by overriding the ``configureLayoutPosition()`` method to return a two-element integer array corresponding to the desired layout column and row.  If left defaulted, Shuffleboard will automatically position the layout.

Note that this will determine the layout position for *all instances of this class*.  Accordingly, when using this feature, it is recommended to pass in the position values as constructor parameters to your class.  Unfortunately, it is not feasible to provide ``Loggable`` layout positioning through annotations, as this does not integrate smoothly with detection of arrays/lists of ``Loggable`` fields, so some hard coupling to user code is unavoidable here.

Skipping Layouts
~~~~~~~~~~~~~~~~

Sometimes it is not worth giving a ``Loggable`` class its own layout.  To place all widgets from a ``Loggable`` class directly into the container of its parent, override the ``skipLayout()`` method to return ``true``.

Adding Custom Logging
^^^^^^^^^^^^^^^^^^^^^

If Oblog's annotation-supported logging functionalities are ever insufficient, a ``Loggable`` class can be given a custom Shuffleboard logging routine by overriding the ``addCustomLogging`` method.  This allows access to the full ``Shuffleboard`` API while retaining Oblog's inferred tab/layout structure.

Excluding and Re-Including Loggables
------------------------------------

Sometimes a ``Loggable`` class will occur as a field of many other ``Loggable`` classes (such as subsystems that are injected into many different commands).  It then becomes desirable to "pick-and-choose" where the ``Loggable`` will occur in the dashboard, since by default, it will be duplicated in every single place in which it occurs as a field.

To do this, annotate the ``Loggable`` class with the ``Log.Exclude`` or ``Config.Exclude`` annotation.  ``Loggable`` classes excluded in this way will never appear on the dashboard unless they are explicitly re-included with a ``Log.Include`` or ``Config.Include`` annotation on the specific field whose display on the dashboard is desired.

Alternatively, the ``Exclude`` annotations can also be used on individual fields, rather than on the entire class.
