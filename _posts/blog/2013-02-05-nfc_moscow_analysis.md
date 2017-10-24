---
layout: post
title: "NFC Moscow metro tickets analyse"
modified:
categories: blog
excerpt:
tags: [nfc, reverse engineering, C++]
image:
  feature:
date: 2010-06-05T15:39:55
---
# NFC Moscow metro tickets analyse

Some time ago I have received package with NFC reader inside (please don’t tell me that it is already included in most of the new mobile phones, my dream is to buy new Nexus 10  tablet from Google http://www.google.com/nexus/10 with NFC, but in Russia it cost amazing 600 euros). Still, it is possible to use ordinary USB reader that look like this one

![]({{ site.url }}/images/blog/2013-02-05-nfc_moscow_analysis/nfc_reader.jpg)

Generally, what is NFC? NFC – near communication field, the modern technology of wireless connection that work on a very small distances (several centimeters). Many new contactless payment systems (paypass), transport and metro card, identifications are build using NFS. Even new modern European and Russian passports have NFC chips with various data about passport holder inside.

So, lets try to read data using NFS. What can we read? Okey, we are from Moscow, so the easiest way to start is to read underground tickets. It works already for 1, 2, 5, 10, 20 rides. I have not tried 1 months passes yet. Brief description of Moscow metropolican tickets.

So, Moscow metropolitan ticket is actually a Mifare Ultralight http://en.wikipedia.org/wiki/MIFARE#MIFARE_Ultralight_and_Ultralight_EV1 card. These card are very cheap, but you can only store 64 bytes and they do not have any cryptographic security. Moreover, first 4 bytes are used for the system information. Dump the card information and lets try to analyze it

![]({{ site.url }}/images/blog/2013-02-05-nfc_moscow_analysis/nfc_analyze_bytes.jpg)

I used different colors to show bits that I can understand – why do we need them and how does the system use them. These fields are card number (located on the other side of the card), number of unused travels, date is issue, expiry date, ticket type and even turnstile number. By the way, this information is not really that secret, there are several Android programs that can read metro tickets, but I have not find any that can read turnstile number (by the way using this number it is possible to find out the last station where the person entered the underground). I even have a marketing idea – mobile application “Check your husband” =) When your husband comes home too late and tells you that he had too much work in his office, you can check his metroticket – maybe the last station it completely different =)

Interesting things to note: Programmer guys are used to the fact that the data is aligned at least by bytes, but this is not the case, the padding is very strange (see upper screenshot)

Example of my program output

![]({{ site.url }}images/blog/2013-02-05-nfc_moscow_analysis/nfc_dump_information.jpg)

By the way, the program uses libNFC, is open-source and one can download it here https://bitbucket.org/gburanov/nfc_test Ok, now we are able to read information from the card, but is it possible to write information to the card, and is it possible at all – please read next topics.
