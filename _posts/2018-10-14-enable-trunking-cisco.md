---
layout: post
title: Enable Trunking
tags: [networking, cisco]
author: Spencer Riner
comment: true
---
Cisco IOS

Login to the switch. 

''
en
conf t
Interface GigabitEthernet0/1
switchport mode trunk
switchport trunk native vlan 1,2,3
'''
