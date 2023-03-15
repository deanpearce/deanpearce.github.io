---
title: "On Software Quality"
date: "2017-01-14"
categories: 
  - "development"
  - "practices"
---

There are many metrics by which quality can be measured. We can take quantitative as well as qualitative approaches when evaluating systems for quality. As a software developer, I am particularly interested in how to effectively measure code quality. Quality is something that cannot be retroactively applied or evaluated, rather it must be designed into a system. As software developers, we have a responsibility in ensuring our code is written with quality in mind. Let's explore some practical ways to improve code quality while maintaining velocity and reducing overhead work.

#### Write Readable Code

The key to writing high quality software is to ensure that any code you write can be read by another software developer at a later date. Unlike traditional engineering where finalized designs are rarely modified, software by it's nature is intended to be re-written, improved, extended, and adapted. There is little or no performance cost when formatting your code for readability and providing comments for future developers. Another sure-fire way to improve code quality is to ensure that code reviews are built into your process. Constructive peer feedback allows developers to improve their own coding standards and habits. Implemented properly, it helps strengthen teams by allowing for open and honest conversation without fear of repercussion.

Technically there are several tools and practices that should be used by almost every team to manage a baseline for code quality. The first to enforce a coding standard across your project, and I don't just mean spaces versus tabs. Things like brace placement (K&R style and the like), naming conventions, and deciding what work should be allowed in constructors. There are many existing style guides such as [Google's Style Guides](https://github.com/google/styleguide) which can be used as a base for your own style guides. Establishing a standard and a reference in-house also allows some of this work to be automated via critic and linting tools which can automatically format code and point out rules which may have been violated. Beyond code style there are best practices to follow for each language, and for each development style. An example of this would be in object oriented programming, regardless of language or style, concepts like classes having a single concern and subsystem architectures are solid foundations on which to build.

When it comes to personal experience, I've learned over my career that privacy and simplicity may require additional thought now, but definitely pays of later. Concepts like ensuring all loops (except main application loops) are properly bounded. There is almost no case for using unbounded while loops or goto statements. Simply by bounding your application entire classes of errors can be eliminated. Second, complex methods should always be broken down into smaller private methods. I don't stick to the one-page rule-of-thumb, but I do recommend ensuring that your function is only responsible for one piece of logic. Complex methods that change behaviour based on input should likely be different methods and the caller should be responsible for making that distinction. Ensure that any variables and methods that don't need to be exposed as public are explicitly marked as private or protected. It is easier to grant access to data than it is to revoke it. Verifying and tracing a system is much simpler when classes have a single concern and marking unnecessary variables and methods as private.

#### Keep Safety and Privacy In Mind

Developers must also consider the safety of their code, regardless of where it is running. Frequently drivers, services, and applications are compromised as a way to obtain access to restricted data. Modern systems are frequently written as online systems, exposing a rather large attack surface through things such as API endpoints, micro services, and web services. Frequently attackers will go after these for weak authentication (hard-coded credentials, default administrator accounts for various tools), SQL injection (not properly escaping or binding parameters), and out-of-date packages (outdated copies of WordPress, unpatched OpenSSL libraries). Within code, it's worth ensuring that all external data is sanitized before being used, and that we don't have access to more information than expected. For example, when writing an Oracle SQL statement, it's safer to use binding than direct string injection. When you write your query, you can use placeholder binding like this: _select \* from foo where bar = ?_. When you go to execute, you would specify your placeholder variables. This would ensure that the data is properly encoded by your driver and prevent one of the most common attack vectors. Even more subtle things, like auto-generated API endpoints that unintentionally expose variables because they are marked public instead of private. It is also important to avoid logging sensitive data as logs can be compromised as they often have more relaxed permissions on who can read them. Ensure things like session keys and passwords are not logged by your application.

Another paradigm of safety is ensuring that your classes are properly scoped. When other subsystems and services consume your code, they are encouraged to use anything marked as public or friendly. This can lead to unexpected behaviour if you unintentionally leave variables and methods public. When writing tight units of code, it is important to only mark methods you intend to be consumable as public. These include get/set methods, constructors, and action methods on your class. If you need to enable extensible code, consider offering interfaces which can be implemented, or base classes which can be extended. This ensures that your code retains control over key attributes and structure.

#### Cover Edge Cases

One key thing missed by many developers when coding are edge cases. While the happy path through the application is frequently well covered, unexpected or improperly formatted input is frequently missed. When writing methods, particularly those that interface with other subsystems, it's worth ensuring that they are appropriately hardened by considering all potential input. This is accomplished via equivalence class partitioning which allows input to be partitioned into classes of equivalent data. Ideally you would then write one test case for each partition to cover the broad spectrum of input. For example, if a method accepts integers we would want to test for values at the minimum and maximum integer value marks. For strings we would want to test null, zero, one, and and maximum length strings to ensure full coverage. For common objects this could likely be generalized to a pattern since the class partitions would be common to the type. Complex input, such as objects more advanced partitions would need to be devised. When edge cases are covered and tested properly, we can be confident that our code is robust and correct.

Exceptions are also frequently mishandled in code. Methods that can throw exceptions aren't trapped at the appropriate level, or worse they aren't even correctly propagated upwards. This can lead to unstable or incorrect application behaviour. When writing code it is important to consider how exceptions should be handled. In some cases we may only want to handle a subset of errors. For example a configuration class may try to read a configuration file from disk and get a _java.io.FileNotFoundException_ which we can handle gracefully by using application defaults rather than custom values from a file on disk. In another situation we may find a file on disk and try to parse it only to get an _oracle.xml.parser.v2.XMLParseException_ which we may want to pass up to the GUI to let the user know their custom configuration file is corrupt. When writing exception classes, it's worth being verbose so that other classes can decide if an exception should be caught or passed upwards. Well structured and handled exceptions can make entire subsystems more robust and facilitate integration with other system components.

#### Targeted Documentation

This is a fairly controversial topic. Few people like writing detailed documentation despite many developers wishing they had access to cleaner and clearer documentation. Some sources say that the number of lines of documentation should match lines of code. Others say that the code should serve as documentation. My preferred method is to write concise documentation only as needed. If you have followed the previous guidelines for code quality then documentation should act only to clarify complex logic and document API methods within classes and libraries. Examples of this would include covering a basic description for every public method and constructor, and commenting complex regular expressions. When writing, try to stick to unambiguous technical language and use consistent terminology throughout, especially when referring custom components or business-related entities.