---
layout: default
title: How the Internet Works
permalink: /internet
order: 1
---

# {{ page.title }}

![AI-generated illustration of Earth surrounded by a network of lights]({{ "/assets/img/earth-lights.png" | absolute_url }})

Most troubleshooting you will do as a systems librarian boils down to understanding how the internet works, and identifying where problems originate so you can contact the appropriate people for a resolution.

When an undergraduate has difficulty accessing a research database from off campus, is it an issue with their browser settings, their Single-Sign On account, the library's proxy service, or the database vendor? When a faculty member reports that a record in the catalog is "broken," is it a problem with the data in the ILS, a JavaScript or CSS customization, an indexing issue by the catalog host, or something else?

Though these scenarios can seem complex, the process of identifying the causes is straightforward: understand which systems are supposed to talk to each other at each step of the process, and look for breakdowns in the exchange of information. Either the library's proxy service is receiving the expected response from the school's Single Sign-On system, or it's not. Either the catalog host's servers are returning the correct data for a full record display, or they're not.

This lesson will cover the basics of how this information exchange takes place, so you can work out which piece has gone awry in a variety of situations.

## In This lesson
1. [Servers and Clients](#servers-and-clients)
    1. [HTML, CSS, and JavaScript](#html-css-and-javascript)
2. [IP Addresses](#ip-addresses)
3. [Domain Name System (DNS)](#domain-name-system-dns)
    1. [DNS Zones and Records](#dns-zones-and-records)
4. [Requests and APIs](#requests-and-apis)
    1. [Anatomy of a Request](#anatomy-of-a-request)
    2. [Status Codes](#status-codes)
    3. [Request Types](#request-types)
    4. [Synchronous vs. Asynchronous Requests](#synchronous-vs-asynchronous-requests)
5. [Programming Languages](#programming-languages)
6. [Troubleshooting with Browser Tools](#troubleshooting-with-browser-tools)
7. [Learn More](#learn-more)
8. [Citations](#citations)

## Servers and Clients

The modern World Wide Web took shape in the late 1980s at CERN (*Conseil européen pour la Recherche nucléaire,* or the European Organization for Nuclear Research). Computer scientist Tim Berners-Lee proposed linking the many siloed networks of international organizations together to create "a 'web' of notes with links (like references) between them" through **Hypertext.**

From Berners-Lee's "[Information Management: A Proposal](https://www.w3.org/History/1989/proposal.html)," 1989:

> In 1980, I wrote a program for keeping track of software with which I was involved in the PS control system. Called Enquire, it allowed one to store snippets of information, and to link related pieces together in any way. To find information, one progressed via the links from one sheet to another, rather like in the old computer game "adventure"... Enquire, although lacking the fancy graphics, ran on a multiuser system, and allowed many people to access the same data.

Everything we do on the internet is an exchange of data between "clients" and "servers." These terms have the same essential meaning as in colloquial English: when you visit a restaurant as a **client,** you place an order with your **server,** and they deliver the food you requested.

Similarly, when you visit a website, your browser is the client requesting the page from the server, and the physical or virutal machine hosting the website is the server that returns the page as **HTML** (Hypertext Markup Language). Servers can also return data in other formats for programmatic use, which will be covered under [Requests and APIs](#requests-and-apis) below.

A "server" can also refer to one program on a machine. **MySQL** is a common database program that uses client/server architecture. If you want to install a WordPress website, you first need to install and configure the MySQL database server package, and then start the service. While the service is running, you can use the command-line client to tell the service to create databases, query them, update them, delete them, and so on. The future lesson [Intro to Databases]({{ "/databases" | absolute_url }}) will expand on MySQL.

### HTML, CSS, and JavaScript

You might have heard of many different languages, frameworks, and applications used to serve up websites, but they all generate documents with only three essential components for interpretation by a browser: HTML, CSS, and JavaScript.

**HTML** is the document type browsers display as a webpage. Every document consists of a head, with invisible metadata like the title and references to styles and scripts; and a body, with the page contents rendered for users.

(Did You Know: the EPUB format for eBooks is just a compressed folder of HTML documents!)

    <html>
        <head>
            <title>Page Title in the Browser Tab (You Won't See This)</title>
        </head>
        <body>
            <h1>Big Header</h1>
            <p>This is a paragraph.</p>
            <p>
                <a href="https://orbiscascade.org">
                    This is a link to the Orbis Cascade Alliance website.
                </a>
            </p>
        </body>
	</html>

<iframe width="100%" height="150" src="{{ "/assets/html/lesson1-example1.html" | absolute_url }}"></iframe>

**CSS** (Cascading Stylesheets) tells browsers how to render the appearance of webpages. CSS rules define aspects like colors, font sizes, the positioning of elements and the spacing between them.

In the head of HTML documents, CSS can be declared within a ``<style>`` node or a linked file that contains rulesets to apply to all elements that meet certain selection criteria (element name, class, ID, etc.). The style attribute can also be added to individual elements in the HTML body to override global rules.

    <html>
        <head>
            <title>Page Title in the Browser Tab (You Won't See This)</title>
            <style>
                body {
                    font-family: Times, serif;
                }
                p {
                    font-family: Ariel, sans-serif;
                }  
                a {
                    color: #007c89;
                }
                div.box {
                    border: 2px solid #00f;
                    background: #eee;
                    padding: 1rem;
                    margin: 0 0 1rem 0;
                }
            </style>
        </head>
        <body>
            <h1>Big Header</h1>
            <p>This is a paragraph in Ariel font.</p>
            <p>
                <a href="https://orbiscascade.org">
                    This is a link to the Orbis Cascade Alliance website, in teal.
                </a>
            </p>
            <div class="box">
                This element will have a bright blue border
                and a gray background.
            </div>
            <div class="box" style="background: #ffe; border-color: #f0f;">
                The inline style attribute of this element will change
                the background to yellow and the border to hot pink.
            </div>
        </body>
	</html>
	
<iframe width="100%" height="300" src="{{ "/assets/html/lesson1-example2.html" | absolute_url }}"></iframe>

**JavaScript** manipulates HTML documents. For example, scripts can add and remove HTML elements, alter their contents and styling, trigger changes to the page when users click, scroll, or type, etc.

    <html>
        <head>
            <title>Page Title in the Browser Tab (You Won't See This)</title>
            <style>
                #myBox {
                    border: 2px solid #00f;
                    background: #eee;
                    padding: 1rem;
                }
                #myBox span {
                    font-weight: bold;
                }
            </style>
            <script>
                function change_colors() {
                    var box = document.getElementById("myBox");
                    box.style.background = "#ffe";
                    box.style.borderColor = "#f0f";
                    document.getElementById("color1").textContent = "yellow";
                    document.getElementById("color2").textContent = "hot pink";
                    document.getElementById("myBtn").remove();
                }
            </script>
        </head>
        <body>
            <h1>Big Header</h1>
            <div id="myBox">
                This element will have a <span id="color1">gray</span>
                background and a <span id="color2">bright blue</span>
                border around it.
            </div>
            <p>
                <button id="myBtn" onclick="change_colors();">
                    Click to change box colors and remove this button
                </button>
            </p>
        </body>
	</html>
	
<iframe width="100%" height="200" src="{{ "/assets/html/lesson1-example3.html" | absolute_url }}"></iframe>

There are many JavaScript **libraries** and **frameworks** that developers commonly choose for their sites, instead of writing code from scratch. Libraries are sets of functions that can be used to build or enhance any interface, while frameworks have standardized structures for deploying applications quickly. [jQuery UI](https://jqueryui.com/) is a popular library that can be included on any webpage to add widgets and animations without extensive coding.

Most JavaScript frameworks are used for **front end** development, to manipulate HTML on the client. One example is [Angular](https://angular.dev/), which is used in Ex Libris's Alma and Primo NDE products. Some JavaScript frameworks can also be used for **back end** development, to construct web servers. An example is [Node.js](https://nodejs.org/en), which is used in the [Primo Development Environment](https://github.com/ExLibrisGroup/primo-explore-devenv).

## IP Addresses

Computers contact each other by **IP Address.** IP stands for Internet Protocol, the specification for constructing and assigning these identifiers.

**IPv4,** or Internet Protcol version 4, was deployed in the early 1980s. You'll see IPv4 addresses written most often in "dot-decimal notation" with 4 numbers separated by periods. Each of these 4 numbers represents 8 bits, for a total of 32 bits.

    Example: 35.82.168.182
    Decimal: 35       82       168      182
    Binary:  00100011|01010010|10101000|10110110

In IPv4, some address ranges are reserved for **private** use and others for **public** use.

Computers within a local network use private IP addresses to communicate with each other. These addresses begin with 10, 176.16 through 172.31, or 192.168. If a vendor asks for your IP address to whitelist a service, numbers in these ranges won't be useful to them. The machine at 192.168.1.8 on their network will be different from the machine with the same address on your home or organization's network.

To communicate over the internet, your ISP assigns your computer a public IP address, which is globally unique. You can check your public IP address simply by Googling "what is my IP address." There are ways to change the IP address that servers receive and respond to, including proxies and VPNs. These topics will be covered in the next lesson, [Security Basics]({{ "/security" | absolute_url }}).

**IPv6,** Internet Protocol version 6, became the latest standard in 2017. The number of possible unique 32-bit IPv4 addresses, about 4.29 billion, is too small to support the worldwide explosion of internet-connected devices for long. To solve this problem, IPv6 uses a 128-bit space, for approximately 340 undecillion possible unique addresses. (That's 38 zeroes!)

IPv6 addresses are visually distinct from IPv4. Instead of 4 octets separated by periods, IPv6 addresses are written as eight 16-bit hexadecimal values separated by colons. Hexademical represents integers using both the numbers 0-9 and the letters A-F, representing the double-digit numbers 10-15.

    Example: 2600:1f14:1aa9:8f00:6368:4f33:28c5:1f50
    Decimal: 2600             1f14             1aa9             8f00             6368             4f33             28c5             1f50
    Binary:  0010011000000000|0001111100010100|0001101010101001|1000111100000000|0110001101101000|0100111100110011|0010100011000101|0001111101010000

Addresses with fewer than eight values indicate **subnets** within a network. For example, the subnet for Orbis Cascade Alliance web servers is 2600:1f14:1aa9:8f00::/64, which represents 256 possible IP addresses that could be assigned to a new virtual machine in AWS.

Not all ISPs support IPv6 yet. According to [Google's IPv6 Statistics page](https://www.google.com/intl/en/ipv6/statistics.html), only about 45% of their users access services over IPv6 as of August 2024. If an ISP doesn't support IPv6, their customers can't use those addresses to contact servers. If in the course of your systems librarian job you set up a new web server for your patrons, keep in mind that they might not have IPv6 at home, so you'll need to support both protocols simultaneously.

## Domain Name System (DNS)

Remembering long IP addresses for every website would be a nightmare, so in the 1980s the Domain Name System (DNS) was developed. DNS is like an old-fashioned phone book. Phone books listed the names of people and businesses in town with the numbers where they could be reached. Simiarly, DNS associates human-friendly hostnames with the IP addresses of their servers, so users can type *wikipedia.org* into their browsers instead of 198.35.26.96.

On Windows, you can check a domain's IP address(es) in the command prompt with `nslookup.` The command `nslookup orbiscascade.org` will return the result below, showing the IPv6 and IPv4 addresses of the server for the Orbis Cascade Alliance website on AWS.

    Name:       orbiscascade.org
    Addresses:  2600:1f14:1aa9:8f00:6368:4f33:28c5:1f50
                35.82.168.182
				
Before DNS, maps of hostnames and their IP addresses were recorded in HOSTS.TXT files. Hosts files still exist on PCs and can be used to define domains that don't yet exist on the internet, so they'll work on that machine only. On Windows, this file lives at C:\Windows\System32\drivers\etc\hosts. For example, if you need to test a new hosted service for the library and know the IP address from the vendor, you can add the domain you intend to use to this file. Then you can visit the domain in any browser on that computer.

    # Copyright (c) 1993-2009 Microsoft Corp.
    #
    # This is a sample HOSTS file used by Microsoft TCP/IP for Windows.
    #
    # This file contains the mappings of IP addresses to host names. Each
    # entry should be kept on an individual line. The IP address should
    # be placed in the first column followed by the corresponding host name.
    # The IP address and the host name should be separated by at least one
    # space.
    #
    # Additionally, comments (such as these) may be inserted on individual
    # lines or following the machine name denoted by a '#' symbol.
    #
    # For example:
    #
    #      102.54.94.97     rhino.acme.com          # source server
    #       38.25.63.10     x.acme.com              # x client host

Taking this approach, you will encounter browser warnings about potential security risks. You would also see these warnings if you attempted to visit a website in a browser by its IP address instead of its domain. This is because neither a made-up domain nor the raw IP address matches a valid security certificate on the server. The next lesson, [Security Basics]({{ "/security" | absolute_url }}), will address certificates and HTTPS.

### DNS Zones and Records

Your organization's IT department likely maintains the **DNS zone(s)** for your organization's domain(s). These files live on **nameservers** that are defined when the organization purchases the domain from a registrar. Zone files contain several types of records to direct traffic to the appropriate servers.

**A and AAAA records** associate a domain with its IPv4 and IPv6 addresses, respectively. The *orbiscascade.org* zone contains A and AAAA records for the main website, plus subdomains like *archiveswest.orbiscascade.org.*

    @            IN A    35.82.168.182
	@            IN AAAA 2600:1f14:1aa9:8f00:6368:4f33:28c5:1f50
    archiveswest IN A    35.86.54.0
	archiveswest IN AAAA 2600:1f14:1aa9:8f00:c596:7c87:b1:cd0e

**CNAME records** associate a domain with another domain, rather than an IP address. When setting up hosted services to use your institution's domain name, such as a *librarysearch.myschool.edu* for a discovery interface or *research.myschool.edu* for LibGuides, vendors will commonly instruct you to ask your IT department to set up a CNAME record.

For example, GitHub Pages hosts this course website, but visitors use *github.orbiscascade.org* to access it. A CNAME record in the orbiscascade.org zone forwards requests for github.orbiscascade.org to orbis-cascade-alliance.github.io. You could replace the URL in this browser with *https://orbis-cascade-alliance.github.io/systems-librarianship/,* and this site will load just the same.

    github IN CNAME orbis-cascade-alliance.github.io.

Other record types are used for email routing and authentication, website ownership verification, and more niche applications. Some will be shown in the next lesson, [Security Basics](security).

<!--## Requests and APIs

### Anatomy of a Request

### Status Codes

### Request types

### Synchronous vs. Asynchronous Requests

## Programming Languages

(Include server-side vs client-side)

## Troubleshooting with Browser Tools-->

## Learn More

These W3schools tutorials are a good starting point to learn the basics of HTML, CSS, and JavaScript.

- [HTML](https://www.w3schools.com/html/)
- [CSS](https://www.w3schools.com/css/)
- [JavaScript](https://www.w3schools.com/js/)

## Citations

The image at the top of this page was generated by the Adobe Firefly engine.