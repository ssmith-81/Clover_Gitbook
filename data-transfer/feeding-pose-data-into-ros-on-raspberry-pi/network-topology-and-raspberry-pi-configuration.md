---
description: >-
  This page outlines how to reconfigure the raspberry Pi to use an external
  network instead of its own.
---

# Network Topology and Raspberry Pi Configuration

A better network connection topology is having all the machines working together connected over the same network. The Clover's Pi originally is setup to provide its own network through the access point mode therefore it must be reconfigured to connect to the wifi router and communicate with the other machines such as the motion capture system computer. This reconfiguration process can be found on the [Clover website ](https://clover.coex.tech/en/network.html#switching-adapter-to-the-client-mode)where the steps are repeated here:

1.  Disable the `dnsmasq` service.&#x20;

    <pre class="language-shell-session"><code class="lang-shell-session"><strong>sudo systemctl stop dnsmasq
    </strong>sudo systemctl disable dnsmasq
    </code></pre>
2.  Enable DHCP client on the wireless interface to obtain IP address. In order to do this, remove the following lines from the `etc/dhcpcd.conf` file:&#x20;

    ```
    interface wlan0
    static ip_address=192.168.11.1/24
    ```
3.  Configure `wpa_supplicant` to connect to an existing access point. Change your `/etc/wpa_supplicant/wpa_supplicant.conf` to contain the following:&#x20;

    ```
    ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
     update_config=1
     country=GB

     network={
         ssid="SSID"
         psk="password"
     }
    ```
4. where `SSID` is the name of the network, and `password` is its password.
5.  Restart the `dhcpcd` service.

    ```
    sudo systemctl restart dhcpcd
    ```

{% hint style="info" %}
To configure the wpa\_supplicant.conf file use sudo for permission. Also, the permanently set static IP address of the Pi has been turned off so in order to ssh connect to the Pi. You need the one assigned when it is connected to the wifi router. This can be found by connecting the Pi to a monitor, and using a keyboard login with:

* Username: pi
* Password: raspberry
{% endhint %}

Then type:

```
ip a
```

You'll get a screen like the following figure with the highlighted IP.

<figure><img src="../../.gitbook/assets/ip_a_shot_edit (1).png" alt=""><figcaption><p>Determine ip of rapberry Pi when connected to wifi router</p></figcaption></figure>
