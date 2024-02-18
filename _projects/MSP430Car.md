---
title: "A model car controlled by MSP430 capable of autonomous navigation"
permalink: /projects/msp430car
excerpt: "<b>#Sensor #Hardware Design #Microcontroller #Greedy Algorithm</b><br/>A model car using sensors and Bluetooth to obtain contextual information, and utilizing MSP430 to make autonomous navigation decisions.<br/><img src='msp430car.jpg' width='260' height='200'>"
---


**[This repository](https://github.com/zhaosheng-thu/msp430car)** implemented a microcontroller-controlled car capable of autonomous navigation and accessing virtual resources.

- We utilized distance sensors and color sensors to gather contextual information about the environment in which the car operated.

- We communicated information with the upper computer using a custom-designed Bluetooth communication protocol.

- The movement of the car was controlled by an MSP430 microcontroller, which operated servos to maneuver the car.

- Using a greedy algorithm, we efficiently navigated the 11x11 field to gather the maximum resources in the shortest time possible.

<div style="text-align:center;">
    <iframe width="200" height="120" src="https://www.youtube.com/embed/n2sC8Pn0ipk" frameborder="0" allowfullscreen></iframe>
</div>

With collaboration from our team members, we achieved the **championship** in the TI Cup.
