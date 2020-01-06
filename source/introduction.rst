Introduction to Oblog
=====================

What Is Oblog?
--------------

Oblog is an annotation-based Shuffleboard telemetry logging utility for FRC teams.  With Oblog, logging your robot state to Shuffleboard is relatively painless and has a minimal code footprint - ideal for teams that want the benefits of extensive telemetry logging without the hassle of designing their code around it.

Oblog is currently only available for Java, though it will also work seamlessly with Kotlin with no additional configuration.

Why Use Oblog?
--------------

`Shuffleboard <https://docs.wpilib.org/en/latest/docs/software/wpilib-tools/shuffleboard/getting-started/shuffleboard-tour.html>`__ is the supported WPILib dashboard and telemetry-logging solution.  Shuffleboard has many extremely convenient features, including tab-based display, an assortment of layout types, event logging, and more.  It is a fantastic solution for both dashboard telemetry display and telemetry logging.

However, the Shuffleboard API provided through WPILib is highly verbose and somewhat clunky.  The fluent builder pattern used results in rather long method call chains for relatively simple dashboard configuration, and widget parameters are specified entirely through string-valued key-value pairs, which are error-prone and not self-documenting.

Oblog improves this by replacing the fluent builder API with one based on `annotations <https://en.wikipedia.org/wiki/Java_annotation>`__.  Rather than calling methods to log data explicitly, Oblog allows users to tag fields, getters, and/or setters with widget-specific annotations, which are then automatically sent to the dashboard.  This results in an extremely small code footprint, and allows logging functionality to easily be added to existing code with no major changes - simply annotate the fields you want to be logged, and Oblog does the rest.

For complex robot projects, such as those using the WPILib Command-based framework, it often becomes desirable to structure both the dashboard layout and the log file structure to mirror the structure of the code.  Oblog supports this, as well - through use of the ``Loggable`` interface, users can specify classes that will constitute tabs or layouts on their dashboard.  Oblog will automatically recurse down the tree of ``Loggable`` objects at start time, building a corresponding tree of tabs, layouts, and sub-layouts on the dashboard.  Users can thus, for example, easily specify separate tabs for each of their robot subsystems or commands, without significantly increasing their code verbosity - the annotations remain unchanged, and the annotated fields are automatically placed in the correct tabs or layouts!

Contributing to Oblog
---------------------

Oblog is an open-source project, and suggestions/contributions from users are welcome on the `github page <https://github.com/Oblarg/Oblog>`__.
