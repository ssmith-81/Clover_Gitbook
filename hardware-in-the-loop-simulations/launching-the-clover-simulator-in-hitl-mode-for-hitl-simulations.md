# Launching the Clover Simulator in HITL Mode for HITL Simulations

The first step is setting up the Clover simulation environment, this tutorial is based on HITL simulations so it assumes you already have this setup. It is preconfigured in the [Clover virtual machine](https://clover.coex.tech/en/simulation\_vm.html#simulation-vm-setup) or can be setup in an Ubuntu environment with the [native installation](https://clover.coex.tech/en/simulation\_native.html#native-setup).

For those who are less interested in how the setup works and more interested in just using the HITL simulation platform with the Clover, the roslaunch command is all that is needed to launch the simulation in HITL mode when the flight controller is connected to the machine via usb:

```
roslaunch clover_simulation simulation.launch mode:=hitl
```

{% hint style="warning" %}
At the moment the above command will only work if the Clover workspace environment is setup with the [hitl branch](https://github.com/CopterExpress/clover/tree/hitl). The changes to be discussed in the following section have not been merged to the master branch. If they are not in your setup, follow along and make the appropriate changes to the specified files to allow for an easy HITL launch the the Clover platform.&#x20;
{% endhint %}

The first step is setting up the Clover simulation environment, this tutorial is based on HITL simulations so it assumes you already have this setup. It is preconfigured in the [Clover virtual machine](https://clover.coex.tech/en/simulation\_vm.html#simulation-vm-setup) or can be setup in an Ubuntu environment with the [native installation](https://clover.coex.tech/en/simulation\_native.html#native-setup).
