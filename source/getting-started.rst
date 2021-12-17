Getting Started with Oblog
==========================

Integrating Oblog with a robot project is easy - this page will provide a brief guide on the steps required to do it.

Adding Oblog as a Dependency
----------------------------

The first step to using Oblog is to add it to your robot project as a dependency.  Oblog is distributed using `jitpack <https://jitpack.io/>`__.

To add Oblog as a dependency, we only need to add a couple lines to your project's ``build.gradle``.  First, add the following:

.. code::

  repositories {
    maven { url 'https://jitpack.io' }
  }

Secondly, add the following to the ``dependencies`` list:

.. code::

  implementation "com.github.Oblarg.Oblog:RELEASE_TAG"

where ``RELEASE_TAG`` is the `latest release version tag <https://github.com/Oblarg/Oblog/releases>`__ (e.g. ``3.0.3``).

If you wish to automatically build with the latest version of Oblog, you can use ``master-SNAPSHOT`` instead of a release tag - keep in mind, however, that this may break your code without warning when Oblog is updated.

Configuring the Logger
----------------------

Now that Oblog has been added as a dependency, it's time to start logging!

The first thing to do is to determine which project class will serve as your "root container."  This is the "base class" of your robot, and will constitute the primary tab of your dashboard.  For a basic robot project, this will likely be ``Robot.java``.  For a command-based project, it will likely be ``RobotContainer.java``.  For the purposes of this guide, we'll assume a simple project is being used, and so ``Robot.java`` is the root container.

Now, we'll add the following call to the ``robotInit`` method of ``Robot.java`` (note: you will need to import the ``Logger`` class from the ``io.github.oblarg.oblog`` package):

.. code:: java

  // The first argument is the root container
  // The second argument is whether logging and config should be given separate tabs
  Logger.configureLoggingAndConfig(this, false);

The ``configureLoggingAndConfig`` call takes the aforementioned "root container" as a parameter (since we've chosen ``Robot.java`` as our root container, we simply pass ``this`` since this call is located in ``Robot.java`` itself).

Finally, we add the following call to the ``robotPeriodic`` method of ``Robot.java``:

.. code:: java

  Logger.updateEntries();

The logger is now configured, and we're ready to do some basic logging.

Logging a Field
---------------

To log a field in the root container, simply annotate it with the ``@Log`` annotation (imported from the ``io.github.oblarg.oblog.annotations`` package):

.. code:: java

  @Log
  int exampleField = 5;

The field will be automatically added logged on the dashboard, and will track the value of the field in code.  It's that easy!

Of course, this is only a very simple example.  For a description of the full set of logging features supported by Oblog, see :ref:`logging:Oblog Log Annotations`.

Configuring a Field
-------------------

Oblog doesn't only support logging - it also supports configuring the value of fields from the dashboard!  To bind a value of a field to an interactive dashboard widget, simply annotate a setter function for the field with the ``@Config`` annotation:

.. code:: java

  @Config
  public void setExampleField(int value) {
    exampleField = value;
  }

Oblog will automatically call the setter with the new value any time its value is changed on the dashboard!

The ``@Config`` annotation can also be used directly on fields that implement the WPILib ``Sendable`` interface.  For a full description of the config features supported by Oblog, see :ref:`configuration:Oblog Config Annotations`.

Creating Additional Tabs
------------------------

As our robot program becomes more complex, it becomes less and less tenable to just log everything in the root container's tab.  Oblog's solution to this problem is to automatically infer the tab structure of your dashboard from the structure of your robot code.  To enable it to do this, we use the ``Loggable`` interface.  Any field of your root container that implements the ``Loggable`` interface will automatically be given its own Shuffleboard tab.

For an in-depth description of the use of the ``Loggable`` interface, see :ref:`loggables:Creating Loggable Classes`.
