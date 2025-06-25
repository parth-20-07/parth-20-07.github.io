---
id: 
aliases:
  - Software Architecture for Projects
tags:
---


# Software Architecture for Projects

> Guide: How to Create a Software Architecture

## Base Structure

A standard and modular software Architecture would look as presented below:

```mermaid
flowchart LR

A[Statement] --> B[Function]
B --> C[Module]
C --> D[Architecture]
```

We need a proper architecture in our code as our development should not focus on one time run. We create things that need to be checked, edited and reformed. If we create a single use code, we will run into the issue of needing to remake the complete code everytime the change needs to be done because our earlier approach resulted in a clunky and jumbled piece of code.

## Why a reliable architecture required

A good architecture can have with: 

- **Reasonability**: With limited brain power that we have, we want the code flow to be simpler to understand. This can be done by combining similar code together into a single file or module and making processes as human readable as possible by converting a group of statements into smaller functions. 
- **Maintainability**: Since the code is divided into human readable format, if we need to make certain changes, moving around and making changes becomes easier. 
- **Collaboration**: Easy to read code makes it easier for people to pitch in and work with it. 
- **Reusability** 
- **Portability**: Modular code makes it easier to transfer code between different projects easily. If you seperate platform dependent and platform independent code, the only task left is to change the platform dependent code when you switch platform. 
- **Testability**: Testing individual process clubbed as function make it easier to test rather than testing the complete software at a single stretch where you might be not able to catch edge cases or find exact location of fault in code.

A well developed software architecture might look somewhat as shown below:

![https://www.notion.so/Software-Architecture/Sumobot.png](https://www.notion.so/Software-Architecture/Sumobot.png)

Sumobot Architecture

- Driver are hardware dependent code where changes will be needed to be made in order to change platforms
- Application are hardware independent code which can be ported without significant changes.

This architecture can be made once you have an idea of the requirements you have based on the sensor/actuator requirements along with their communication/interface protocols required for the product to
work.

## What to keep in mind while making a good architecture

A good architecture will have following things in practise: 

- Decoupling: Decouple code as much as possible to seperate hardware-dependent code from application layer code. 
- Modularity 
- Seperation of Concerns 
- Single Responsibility 
- Cohersion 
- Encapsulation 
- Don’t repeat yourself (DRY): Make sure that the code you make is not duplicate. A single change at a location of code should be sufficient to implement those changes across the project rather than making change in multiple files.

## Tips

- [Embedded Artisary: Embedded Systems Architecture Resources](https://embeddedartistry.com/blog/2019/07/12/embedded-systems-architecture-resources/)
- Efficient Implementation of printf/sprintf for Embedded Systems: [mpaland/printf](https://github.com/mpaland/printf)

