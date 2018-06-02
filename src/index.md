# WonderWp Framework Documentation

This is a WordPress **framework** whose goal is to give WordPress **industrial and modern development capabilities**.

## Modern development capabilities for WordPress

What are the characteristics of a modern php code base? Do we have those in WordPress by default? Do we have those by adding WonderWp?

| Characteristic        		| WordPress alone           | with WonderWp  |
| ------------- 					|:-------------:      | -----:|
| Object Oriented Programming | Born procedural, some OOP implementation | OOP by Design |
| Composer | &times; Absent      |    ✔ Present |
| PHP Fig recommendations | &times; Not implemented      |    ✔ Implemented |
| PSR-4      | &times; Not supported      |   ✔ Supported |
| Autoloader | &times; Not provided      |    ✔ Provided by composer <br /> (based on PSR-4) |
| Dependency injection      | &times; Not supported      |   ✔ Supported by a Container |
| Package management | &times; Not provided      |    ✔ Provided by composer |

As you can see, by default WordPress alone does not really have many characteristics of a modern php codebase. 

The WonderWp framework's aim is to bridge that gap and enhance the developer experience. By doing so, we also make a step towards other frameworks and libraries by providing serveal PHP-FIG recommendations implementations, and opening the use of composer.

## What does WonderWp add to WordPress?

WonderWp doesn't recode WordPress, it doesn't really add core concepts that WordPress doesn't have either. Instead, it provides classes that sit on top of WordPress procedural concepts to bring developers a more modern architecture.

### Overview

- It helps you to work with **composer**, gives you access to its ecosystem of libraries and allows you tu use its **Autoloader**.
- It gives you a **Dependency injection container**
- It provides classes to convert procedurals concepts into **Object Oriented** Interfaces, and PSRs, and implementations for :
	- Routing
	- Caching
	- Forms
	- HTTP foundation
	- Logging
	- Emails
	- Medias
	- Notifications
	- Post Panels
	- Post Types
	- Search Engine
	- CLI
- Instead of writing 1000 lines long functions files, it encourages you to declare **services**, aka classes to work with :
	- Activator / Deactivator
	- Routing
	- Assets Management
	- Ajax endpoints
	- Shortcodes
	- WP-CLI commands
- It gives **MVC** capabilities
- IT proposes a **plugin blueprint** (folder organisation, naming conventions, full object approach, empty function.php file) 

## Who is it for?

Probably not everyone.

This framework could interest you if

- You are willing to adopt an industrial and modern development process.
- You are faced with projects that are a true fit for a CMS but also include serious code challenges.
- You have industrial development needs such as high production volumes, repeatable solutions, in house production teams. 
- You are looking at capitalizing on previous projects to build up reusable plugins and themes. Plugins and themes produced with wonderwp are meant to be full object, consistent, predictable, composerable, testable, and durable.
- You are used to frameworks and less to CMSs
- You are an agency, a development team or a freelance person who have in house processes.

On the other hand, if you are a plugin developer who's aim is to make a plugin usable by the widest WordPress audience, it could be a cost you migh consider too big to have WonderWp and composer as dependencies.

## In a bit more depth

<!-- #### Composer

We wanted to make sure our WonderWp based work could play well with composer. This framework is therefore on packagist here : [https://packagist.org/packages/wonderwp/framework]()

You can install it like this:

```
composer require wonderwp/wonderwp
```

For the moment, the framework is only installable via composer but we've planned to release a plugin version as well in the future.

For a composer based WordPress architecture, we recommend [https://roots.io/bedrock/]().

Composer also embarks an autoloader to avoid the need for requiring files everywhere in your plugins. The framework encourages you to follow the PSR4 recommendation and to interact with the autoloader. -->

### Services 

Trying to follow good object oriented principles, we encourage developers throught the use of this framework to adopt a few concepts.

- Full object plugins, no procedural code
- One object per type of task, aka services, which aim is to have one responsibility only. This applies to Hook management, shortcode management, assets management, routing managements, shortcode management and so on.

### Dependency Injection

We wanted our code to be as modular as possible and bring dependency injection capabilities to WordPress.

When working with a dependency injection container, you open up the possibility to switch the object that's going to be used behind the key you request from the container as long as it implements a given interface. Because if it does, you'll be sure that is object exposes the public methods required for your code to run properly.

That's why in addition to add a DI container, we've also **added** many **interfaces** to work with WordPress core concepts, and wider programming concepts, (Emails, Routing, Logging, Caching)...

### MVC

Following the full object philosophy, shortcodes or custom routes resulting in a public or admin output are managed by controllers. Controllers work with services to get the data they need, and pass that to views. Views do not have a templating engine for the moment by default as dependency in this framework, but you could easily add your own. 

### Plugin blueprint

This framework proposes a way of organizing folders following the PSR4 recommendation for classes, an admin folder for your plugin admin side, and a public folder for your plugin public side, but nothing's sompulsary, you can do as you whish.

There's also a plugin generator to help you quick start your plugin development.

## Sounds interesting?

- Getting started
- Creating a plugin
- Framewotk components
