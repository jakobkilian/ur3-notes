# Notes on Using UR3 for Creative Applications

Controlling a Universal Robots UR3 is far from trivial, especially when it comes to performing smooth, real-time movements based on sensor input. In addition to the inherent complexity of programming crash-free 3D movement UR lacks proper documentation and sometimes provides buggy installers. 
**In this repo I want to share notes and findings of my experiments with the UR3 to give other people an easier start.**
These notes were made as part of my work at KISD, Cologne, and refer to the UR3 model that was available in my lab there. In principle, they can be applied to other UR models.

## Universal Interface via OSC/MQTT over Network
UR offers a [bunch of possibilities to interface with the UR3](https://www.universal-robots.com/articles/ur/interface-communication/overview-of-client-interfaces/). I looked through this list, but they all had drawbacks in terms of compatibility and universality. E.g. "RTDE" required installations on the client side – some having issues about the installation which haven't been solved for years at the time. On top connecting to UR controller by default only works with Ethernet and fix IP addresses. Having e.g. microcontrollers in a WiFi network to control the robot would be tricky in this setup. So I wondered why there wasn't a easy-to-use universal interface that could be used to remotely control the robot from any programming language and any device via network. That's why I build a OSC/MQTT bridge to control the robot from within e.g. python, MaxMSP, Processing or others on any device in a network (Ethernet/WiFi) set up by an additional intermediate Raspberry Pi. This worked very well for my use cases and can hopefully be rebuild by following the instructions in the repository

![u3-bridge-simple-cut](https://github.com/user-attachments/assets/43a603ab-c351-47a9-8466-c7b3662ca425)

## Exemplary Applications of the Bridge

<details>

<summary>Fluent Real Time Movements</summary>

A main goal of my experiments was to achieve fluent movements based on real time input from e.g. sensors.

In most industry-standard applications for robots of this type, the start and end points of movements and their timing are known. Depending on the application, commands such as movej or movel are used, which allow the arm to be moved with parameters such as _time, acceleration, maximum speeds, and interpolating blends between points_. The principle is always the same: start at A, accelerate according to the parameters, slow down, and stop at point B. However, if the robot is supposed to follow a hand gesture or move along vector paths in a smooth, uninterrupted motion, it becomes more tricky: sending many individual points would cause it to stop at each one. We need to use the servoj to acheive this and send new points at a rate sufficient to not hear or see the "frames". In this repository I wrote up some principles I learned, in order to create something like this:



https://github.com/user-attachments/assets/54c13f17-3321-4f9e-9587-f5d41fd7ec33

</details>
