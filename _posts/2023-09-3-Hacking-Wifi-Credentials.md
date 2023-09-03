---
layout: post
title: Hacking Wi-Fi Credentials
---

### Extracting Wi-Fi Credentials Using Command Line

## Introduction

In our modern digital era, having access to Wi-Fi connectivity has become a fundamental necessity to stay connected to the internet, whether you're at home, in the office, or traveling. It's worth noting that Wi-Fi network credentials are typically stored in clear-text format, which means they can potentially be extracted or viewed under certain circumstances. When we successfully compromise the host during pentest engagements, it's crucial to investigate the system for stored Wi-Fi credentials. The user may employ the same Wi-Fi password for their AD account.

## Section 1: Getting Started

Before we dive into it, let's ensure that you have everything you need to get started.

### Prerequisites

To follow along with this tutorial, you'll need:

- A Windows computer with administrative privileges.
- Basic familiarity with the Windows operating system.
- CMD/PowerShell.

Now that we have the essentials in place, let's begin exploring how to list available Wi-Fi networks/profiles using CMD/PowerShell.

## Section 2: Listing Available Wi-Fi Profiles

To interact with Wi-Fi profiles through CMD/PowerShell, we'll be using the `Netsh` (Network Shell) command, a versatile tool for network configuration and management. Here's how to use CMD/PowerShell to list all available Wi-Fi profiles :

- Open CMD/PowerShell with administrative privileges
- To list available Wi-Fi networks/profiles, run: ```netsh wlan show profile```

![_config.yml]({{ site.baseurl }}/wifi-images/01 netsh wlan.png)

## Section 3: View Wi-Fi Profile Information

To view detailed information about a specific Wi-Fi profile, type the following command, replacing "Wi-Fi-Profile" with the name of the network/profile you want to investigate:
```netsh wlan show profile "profile name" ```

![_config.yml]({{ site.baseurl }}/wifi-images/02 detailed profile.png)

## Section 4: View Wi-Fi Passwords in clear-text

To view the password for a specific Wi-Fi network/profile, use the following command, replacing `"Wi-Fi-SSID"` with the name of the network you want to check:
```
netsh wlan show profile "profile name" key=clear```

 - Look for the "Key Content" field under the "Security settings" section. This will display the Wi-Fi password in clear text.

![_config.yml]({{ site.baseurl }}/wifi-images/03 clear text credenials.png)

## Section 5: Script to extract all the Wi-fi credentials stored in clear-text

- I found handy script on github `WifiPasswordExtractor.ps1` to extract all the clear-text passwords.

```PS C:\WINDOWS\system32>.\WifiPasswordExtractor.ps1```

![_config.yml]({{ site.baseurl }}/wifi-images/04 wifi script.png)

References:
[WifiPasswordExtractor](https://github.com/sambiyal/WifiPasswordExtractor)

<!--Below comment is the example of using images in the blog-->

<!-- [_config.yml]({{ site.baseurl }}/wifi-images/01 netsh wlan.png)] -->

