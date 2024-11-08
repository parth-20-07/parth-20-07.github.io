---
id: 1_Picking_the_Parts
aliases:
  - 1. Picking Parts for System
tags: []
---


# 1. Picking Parts for System

> Guide: Picking the Parts for a Small Robot

1. **(Idea/Goal)** List the most essential tasks for the design with the goals in mind. These are generally given by the client based on the end goal that the product needs to achieve. e.g. For a Sumo Robot, the tasks are:
    1. Locate Enemy
    2. Push Enemy Out
    3. Stay inside dojo

    ![[1_idea.png]]
    
2. **(Requirements)** Set down rigid requirements for the design. These are generally given by the client based on their feasibility or usage e.g.
    1. Side of design
    2. Autonomous
    3. Remote Start
    4. Cannot damage other robots/platform
    5. No suction
    6. Battery Life should minimum be 3 mins
        
    ![[1_requirements.png]]

3. **(Design/Part Selection)** Based on the goals, look for the actuators/sensors needed as input/output. Based on the needed sensors and actuators, decide on the MCU that can handle the required peripheral load and matches the power ratings.
    
    A thought process can look like this when you sketch it out:
    
    ```mermaid
    flowchart LR
    
    - [ ] A[Which Motor?, How Many?] --> B{What type of motion do I need to perform}
    B --> |Precise| C(Servo Drive)
    C --> |Non-Precise| D(Continuous Drive)
    
    E[Power to Device] --> F{Device is stationary?}
    F --> |Stationary?|G{Constant Supply available?}
    F --> |Mobile?|H(DC)
    G --> |Yes|I(AC)
    G --> |NO|J(DC)
    ```
    
    ***(This is just a sample thought process and this needs to be extended much furthur to choose each parts like sensor types and exact model based on requirements.)***
    
    ![[1_design.png]]
    
4. Take a look at the peripherals and their datasheet to know the safety and operating power ranges (V, I and P all included) and search for interfacing circuits between MCU and Peripherals in case of different standards or limits.
5. Factors to choose MCU:
    1. Peripheral Requirments: How many GPIO/I2C/SPI, etc are needed for basic design?
    2. Speed of the controller: Precise operation needs processors with higher frequency than less/none precise operations.
    3. Code Size: Large Size of codes will require higher amount of Flash Memory
    4. Community Support: Well documented MCU are better in terms of development as it helps in troubleshooting when you are stuck during development as many might have too before you and would had found solutions already.
    5. Power: How much power does it consume and is the voltage levels too different from other peripherals such that this would need a special power circuit?
    6. Price: For non-critical option, cheaper the better. For critical operations, reliability, quality and security of device functioning matters.
    7. Stock: You want the chip to be easily accessible anytime you want.
    8. Tools: Available tools to develop the project around the MCU.

> Tips: If you don’t have much experience with design

**Prototyping with experimentation > Prototyping with Experience**.
