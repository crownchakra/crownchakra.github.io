---
layout: post
title: Hacking Wi-Fi Credentials
---
<!--

# Hacking Wi-Fi Credentials Using Command Line
-->
## Introduction

In today's digital age, Wi-Fi connectivity is essential for staying connected to the internet, whether at home, at work, or on the go. Windows operating systems store Wi-Fi network credentials to make it easier for users to reconnect to known networks.

## Section 1: Getting Started

Before we dive into it, let's ensure that you have everything you need to get started.

### Prerequisites

To follow along with this tutorial, you'll need:

- A Windows computer with administrative privileges.
- Basic familiarity with the Windows operating system.
- CMD/PowerShell.

Now that we have the essentials in place, let's begin exploring how to list available Wi-Fi networks using CMD/PowerShell.

## Section 2: Listing Available Wi-Fi Networks

To interact with Wi-Fi networks through CMD/PowerShell, we'll be using the `Netsh` (Network Shell) command, a versatile tool for network configuration and management. Here's how to use CMD/PowerShell to list all available Wi-Fi networks:

- Open CMD/PowerShell with administrative privileges
- To list available Wi-Fi networks, run: ```netsh wlan show networks```

## Section 3: View Wi-Fi Profile Information

To view detailed information about a specific Wi-Fi profile, type the following command, replacing "Wi-Fi-SSID" with the name of the network you want to investigate:
```netsh wlan show profile name="Wi-Fi-SSID" ```

## Section 4: View Wi-Fi Passwords in clear-text

To view the password for a specific Wi-Fi network, use the following command, replacing `"Wi-Fi-SSID"` with the name of the network you want to check:
```
netsh wlan show profile name="Wi-Fi-SSID" key=clear```

 - Look for the "Key Content" field under the "Security settings" section. This will display the Wi-Fi password in clear text.


<!--Below comment is the example of using images in the blog-->

<!-- [_config.yml]({{ site.baseurl }}/images/config.png)] -->

