The Oblog Logger
================

Oblog's `Logger class <https://oblarg.github.io/Oblog/io/github/oblarg/oblog/Logger.html>`__ is responsible for the "magic" of Oblog.  It performs the initial recursive search for loggable fields and methods, and is responsible for updating the display values of the widgets.

Initial Logger Configuration
----------------------------

As seen in the :ref:`getting started <getting-started:Configuring the Logger>` section, for Oblog to work the user must call one of ``Logger``'s configuration methods at robot startup.

Each configuration method requires the user to specify a "root container."  This is the "root node" from which the recursive search for ``Loggable`` fields will be performed.  In simple terms, this should be the class that contains most of the important robot fields - generally, this will be either ``Robot.java`` or ``RobotContainer.java`` for standard WPILib project structures.

Users with more "distributed" project structures are free to call the configuration methods multiple times on different root containers.

The available configuration methods are summarized below:

Configuring Logging and Config
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Calling the ``configureLoggingAndConfig`` method will parse the object tree for both ``@Log`` and ``@Config`` annotations.  The second parameter is a boolean which will determine whether ``@Log``- and ``Config``-generated widgets are given separate tabs, or combined on the same set of tabs.

Configuring Logging Only
^^^^^^^^^^^^^^^^^^^^^^^^

The ``configureLogging`` method will parse the object tree *only* for ``@Log`` annotations.

Configure Logging Only (NT Only)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The ``configureLoggingNTOnly`` method will parse the object tree *only* for ``@Log`` annotations, and also bypass all Shuffleboard widgets and only push the raw logged values to NetworkTables.  This is useful for teams that wish to use a custom dashboard implementation, or otherwise consume the NetworkTables information through a means other than Shuffleboard.

Configuring Config Only
^^^^^^^^^^^^^^^^^^^^^^^

The ``configureConfig`` method will parse the object t ree *only* for ``@Config`` annotations.

Updating the Logged Fields
--------------------------

Initial logging sets up the Shuffleboard widgets and populates them with initial values; however, in order for the displayed widgets to remain synchronized with the values in code, Oblog needs to periodically update them.

Accordingly, the ``updateEntries`` method must be called periodically from the user program.  Generally, this is done from the ``robotPeriodic`` method of ``Robot.java``.

This *can* be done from elsewhere, but it is *essential* for the purposes of thread-safety that the ``updateEntries`` method be called from the same thread context in which the logged fields are declared.  For complicated user programs with multiple thread contexts, the relevant :ref:`configuration method <logger:Initial Logger Configuration>` should be called separately for each thread context.

Disabling Cycle Warnings
------------------------

The ``Logger`` will automatically terminate a branch of the recursive search when it detects a cyclic reference of ``Loggable`` objects (otherwise it would recurse indefinitely).  By default, it will print a user warning when this happens.

However, some typical design patterns (such as singletons) rely heavily on cyclic references, and so these warnings can become a nuisance.  To disable them, call the ``setCycleWarningsEnabled`` method.
