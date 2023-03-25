Important information for anyone interested in hacking on or contributing to any of the OpenPnP modules.

# Getting Started

Before starting to develop for OpenPnP you should review the [User Manual](User-Manual) to become familiar with the interface and major components of the system.

# Contributing

OpenPnP uses a Fork and Pull style of development. If you would like to submit changes you should fork the project, make your changes and submit a pull request. More information about that process is available at https://help.github.com/articles/using-pull-requests. There is also also specific tutorial made for OpenPnP at https://github.com/openpnp/openpnp/wiki/Your-First-Pull-Request. 

## Licensing and Copyright

OpenPnP uses different licenses for different components. Before submitting changes, please review the license for that module by looking at the [LICENSE file in the root directory](/openpnp/openpnp/blob/develop/LICENSE.txt) of the module and decide if it is acceptable to you. Changes under a different license will not be accepted for merge into the repository.

Contributors submitting large pieces of functionality, i.e. whole source files, whole part designs, major rewrites, etc. may assign their own copyright to the submission as long as you are willing to license it under the existing license for that module.

## Discussion First

If you would like to add a new feature to OpenPnP that changes core or reference functionality, please create a topic on the [discussion group](http://groups.google.com/group/openpnp) to discuss the feature first. Also consider if your feature can be implemented as a new subclass of a Reference class, or an entirely new class, instead of changing or adding to the Reference class. 

## Code Reviews

All pull requests will be reviewed by the maintainers and will either be merged or comments will be provided as to why it's not being merged. You can submit new changes to the pull request as needed if the review uncovers issues.

Note, be sure to use the `test` branch as your base and submit pull requests against `test` (this can be changed after the fact, but it might introduce conflicts, if you started out from the wrong base).

## Granularity

Try to send as little as possible in a given pull request. A single small feature, a single bug fix, etc. The smaller a pull request is the easier it is to integrate into the project and the less likely it is to contain a looked over problem. If you are changing base classes expect significant discussion as to why the change is needed.

## General Usefulness

Your commits should be generally useful, not specifically useful to just your machine or your setup. If the code cannot be shown to be useful to other people, it probably doesn't belong in the main OpenPnP codebase. That's what's private forks are for. In addition, take consideration of the fact that the code will be used in many different configurations. Avoid quick hacks.

## Coding Style

Please try to adhere to the existing code style as much as possible. Understand that your code is likely to be reformatted to fit the project standard if it doesn't follow it. Do not modify the formatting or white-space of _other_ existing code, i.e., make sure that the source code differences (as tracked by git) will be closely restricted to the code regions that you actually changed.

OpenPnP closely follows the [Google Java Style](https://google.github.io/styleguide/javaguide.html). While there may be existing code in OpenPnP that does not follow the style exactly, any new code must follow the style.

There is an Eclipse formatter definition [here](https://github.com/openpnp/openpnp/blob/develop/OpenPnP_Eclipse_Formatter.xml). You can import this into Eclipse and use it to format your code (selectively) to be sure it's following the standard.

Two things in particular are extremely important:

1. [Naming of variables and classes](https://google.github.io/styleguide/javaguide.html#s5-naming).
2. [Always include optional braces](https://google.github.io/styleguide/javaguide.html#s4.1.1-braces-always-used).

Code that does not follow these two guidelines (at least) will not be accepted.

## Describe Your Work

In the pull request comments, explain what the change does. If it is new functionality, explain why it's useful and how it is to be used. If you are fixing an issue, reference the issue number.

See the Pull request template:
https://github.com/openpnp/openpnp/blob/develop/PULL_REQUEST_TEMPLATE.md

## Testing

### Automated Testing (Required)

OpenPnP contains a JUnit based test suite that must pass before anything new can be merged. You can run it with `mvn test` or from your favorite IDE. You should also try to include new tests for your code if possible.

### Manual Testing (Strongly Recommended)

In addition to the automated tests, it's a good idea to run a manual test using OpenPnP's base configuration. To do this:

1. Delete your [configuration directory](https://github.com/openpnp/openpnp/wiki/FAQ#where-are-configuration-and-log-files-located) or start OpenPnP with `-DoverrideUserConfig=true` on the command line. This will reset your configuration to the same configuration that OpenPnP ships with.
2. Start OpenPnP and load the pnp-test.job.xml from the samples directory.
3. Run the job. You should see the camera image move and the machine should simulate picking and placing the entire job.
4. Verify that the job runs and completes without errors. Watch the camera view and see if anything strange is happening.

# Building OpenPnP

## Prerequisites

### JDK or JRE 8+

OpenPnP is written in Java and should run on Java 8 through 15 (inspect the [latest workflows](https://github.com/openpnp/openpnp/actions?query=event%3Apush) for details). 

If you just want to run it you can install the smaller JRE.

If you want to do development on OpenPnP or recompile it, you should install the lowest version (8) JDK in order to tests for compatibility. 

JDKs later than version 15 [have incompatibities that need to be addressed](https://github.com/openpnp/openpnp/issues?q=is%3Aopen+is%3Aissue+label%3Ajdk). Help is welcome! 

You can download the JDKs or JREs at:

- https://openjdk.org/
- http://java.com/getjava

### OpenCV

OpenCV is now included with OpenPnP, so an additional installation is not required. Binaries are included for Windows (x86/x64), Mac (Universal) and Linux (x86/x64).

### Maven

OpenPnP uses Maven for dependencies and building. You can get it at http://maven.apache.org/download.cgi#. See http://maven.apache.org/install.html for additional information on installing it. Once installed, make sure
you can run `mvn --version` from your command line with no errors.

If you are using an IDE with Maven included you don't need to install the command line version. You can find list of modules and dependencies [here](https://sourcespy.com/github/openpnpopenpnp/xx-omodulesc-.html).

## Compiling

To build the entire package so that it can be run from the command line or distributed,
run the command `mvn package`. Once this is complete you can use the `openpnp.sh` or
`openpnp.bat` scripts to start the program.

## IDEs

### Eclipse

You can import the `openpnp` folder into Eclipse as a Maven project.

For additional help with Eclipse see:
* [[Getting-Started-with-Eclipse]]
* [Building the OpenPnP Package with Eclipse](https://www.youtube.com/watch?v=53H_vwrZz6o)

### IntelliJ IDEA v12+

Simply import the `openpnp` folder as a Maven project.
 

# System Architecture

## Framework

OpenPnP is written in Java using [Swing](https://en.wikipedia.org/wiki/Swing_(Java)) and [WindowBuilder](https://eclipse.org/windowbuilder/). Dependency and build management are performed using Maven. The majority of the code has been written using the Eclipse IDE but the system uses no Eclipse specific components and any IDE can be used.

Bindings and property change listeners are used extensively wherever possible. Bindings are provided by JBindings with some custom helper code that is part of OpenPnP.

## System Overview

OpenPnP is made up of 5 core components: Configuration, Service Provider Interface, Model, User Interface and Reference Implementation.

### Configuration

Configuration of OpenPnP is managed with the [Configuration class](http://openpnp.github.io/openpnp/develop/org/openpnp/model/Configuration.html). The Configuration class is used to query and store to two configuration stores.

The primary one is a set of XML configuration files stored under the user's home directory in a subdirectory called `.openpnp`. These XML files are read and written using the [Simple XML](http://simple.sourceforge.net/) framework and are generally pretty transparent to developers. Reviewing the sample configuration files that come with OpenPnP is a good place to start when thinking about adding functionality.

The second configuration store is the user specific Java preferences data store. This is used to store user preferences for the application itself such as window size, units preference, locations of split windows, etc.

The Configuration class is a singleton accessed with Configuration.get() throughout the system. It has a listener system that can be used to be notified of configuration state changes.

In the various machine Java files, you will notice that parameters tagged with `@Attribute` or `@Element` will correspond to an attribute or element in the machine.xml file.  The names of these attributes and elements will vary slightly between the XML and the code.  For example, the source code might be:

`	@Attribute`
`	private double feedRateMmPerMinute;`

whereas the XML would be: 

`      <driver class="org.openpnp.machine.reference.driver.TinygDriver" port-name="COM5" baud="115200" feed-rate-mm-per-minute="3000.0"/>`

The framework used is [Simple](http://simple.sourceforge.net/download/stream/doc/tutorial/tutorial.php) and the style is based on is [HyphenStyle](http://simple.sourceforge.net/download/stream/doc/javadoc/org/simpleframework/xml/stream/HyphenStyle.html).

Basically, in Java, property names are in lower camel case by convention ( http://c2.com/cgi/wiki?LowerCamelCase ) and the XML serializer just splits the words on capital letters and adds a hyphen. So, feed-rate-mm-per-minute becomes feedRateMmPerMinute.

### Service Provider Interface

The [Service Provider Interface (SPI)](http://openpnp.github.io/openpnp/develop/org/openpnp/spi/package-frame.html) is a set of Java interfaces that specify OpenPnP's interface to the real world. Examples of things in the SPI are things like Machine definitions, Camera drivers, Vision Providers, Nozzles, Actuators, etc.

In general, unless you are adding a new type of hardware to OpenPnP you don't need to implement the SPI but in developing features for OpenPnP you will be interacting deeply with objects that themselves implement the SPI.

The Reference Implementation, discussed later, is the core implementation of the OpenPnP SPI.

### Model

Like most MVC based systems, [The Model](http://openpnp.github.io/openpnp/develop/org/openpnp/model/package-frame.html) makes up the domain for data storage in OpenPnP.

The model includes all of the classes that users use to store their data such as Jobs, Boards, Parts, Packages, etc. but the SPI also provides models for storing configuration information.

The root of the model in OpenPnP is the Configuration object and it is responsible for creating all of the rest of the models in the application.

### User Interface

The user interface is the heart of OpenPnP. It starts with `MainFrame` in [the gui package](http://openpnp.github.io/openpnp/develop/org/openpnp/gui/package-frame.html) and branches out from there. Visual representation of user interface components can be found on this [class diagram](https://sourcespy.com/github/openpnpopenpnp/xx-ouiswing-.html).

The user interface is primarily a single window application with 3 main content areas. These are Machine Controls, Cameras, and Jobs and Configuration. 

It should be noted that the user interface is in transition. I am not very happy with the currently model driven user interface and will be working to make it more of a workflow based interface in the future. For the time being, most of the things you see in the user interface map very closely to one of the model classes.

### Reference Implementation

The reference implementation is the default implementation of the SPI. It has two main purposes. The most important one is to be the implementation that runs the actual OpenPnP machine and the second is to serve as an example for people wanting to write their own custom implementations.

Whenever possible, the reference implementation is written to be compatible with a wide class of machines and hardware.

The core of the reference implementation is the [ReferenceMachine class](http://openpnp.github.io/openpnp/develop/org/openpnp/machine/reference/ReferenceMachine.html). Serialization of this class and all it's children becomes the `machine.xml` configuration file. If you want to add hardware to the system you will want to become intimately familiar with this class and the classes it references.

## Component Specifics

### Issues & Solutions

The Issues & Solutions system constantly supervises the machine configuration. It also tracks the progress in initial machine setup using a system of Milestones. It detects missing or faulty configuration and guides the user through an elaborate set of Solutions for interactive and/or automatic configuration and/or calibration. If there is no automatic solution possible, it at least alerts the user to detected Issues. The user remains the boss, proposed solutions can always be dismissed.

The order of these steps is automatically determined through dependencies and Milestones. Prerequisites must be established, before the dependent next steps are proposed. However, where dependencies are not relevant, users are completely free to chose any order of working through them. All the currently ready steps are listed. Most solutions can also be revisited later. Milestones organize these steps into broader intermediate goals for attained machine functionality. 

The supervision is permanent. If users later expands the machine with new components, or introduces a problem through manual (mis-)configuration, or if a new OpenPnP version improves the coverage of Issues & Solutions, the relevant solutions automatically pop up.

Developers can easily integrate Issues & Solutions into their classes.

### Vision

Vision tasks in OpenPnP are handled by Computer Vision Pipelines (`CvPipeline`). For developers and users alike, the Pipeline Editor allows composition of elaborate  pipelines to perform acquisition, processing, conversion, detection, transformation in dedicated pipeline stages. OpenPnP comes with a rich assortment of powerful stages mostly employing OpenCV.

Pipeline can be parametrized from the calling code, providing physical properties to the stages, like previously acquired (auto-)calibration data, expected dimensions, expected relative positions, shapes or template images for subjects to be detected and more. Essential parameters can be exposed to the user for easy use (sliders). 

Proven stock Pipelines are shipped with OpenPnP. Users can copy and edit their own pipelines and associate them with individual Parts, Packages, Feeders, Nozzle tips etc. Pipelines can be exchanged in the community using copy&paste (via XML).

Increasingly, the pipelines are self-tuning and/or fully parametrized from a few easy-to-use exposed parameters. Most physical parameters are provided from the machine configuration and calibration, which is widely supported by Issues & Solutions. Therefore users seldom need to edit pipelines any more.

### Wizards

The concept of a "wizard" is used quite often in OpenPnP. A wizard in OpenPnP is a simple interface that provides a JPanel for display to the user and allows a class to communicate back to a caller. Wizards are typically used for providing user interfaces to configuration tasks and many SPI and Model classes will implement WizardConfigurable, showing that they are able to provide a wizard to be configured.

The Wizard and WizardConfigurable interfaces are provided so that SPI providers can supply user interface without having to modify the core OpenPnP system.

### Job Processing

The [Job Processor](http://openpnp.github.io/openpnp/develop/org/openpnp/spi/JobProcessor.html) is the part of OpenPnP that actually does all the work of picking and placing. When the user starts a Job, the Job Processor takes over and controls the machine until the job is complete.

Job Processor is a bit of a black box from a programming point of view. To learn more about how it works it's best to examine the source code.

# Icon Resources

* https://materialdesignicons.com/
* https://thenounproject.com/

# FAQ

## How can I manage multiple configurations?

By specifying `-DconfigDir` on the command line you can override the default configuration directory for OpenPnP. Example: `-DconfigDir=/Users/jason/.openpnp-openbuilds-machine`.

I manage a number of configurations by creating various Run Configurations in Eclipse. I have one for my primary machine, one for my secondary machine, one for the default directory (no command line parameters) and one that wipes the configuration for the default directory. I use this when I want to reset OpenPnP to it's factory state.

## How can I reset my configuration to the default?

Specify `-DoverrideUserConfig=true` on the command line to overwrite your configuration with the defaults. If paired with `-DconfigDir` you can choose which configuration to overwrite.
