---
title: "Building notification functionality into my notes app"
date: 2021-11-02T23:29:49+13:00
draft: false
tags: [ "internet technologies", "programming", "how-to"]
---

_Big thanks to_ [_binu.nz_](https://binu.nz)_,_ [_matteas.nz_](https://matteas.nz)_, and_ [_pancake.nz_](https://pancake.nz) _for their help on this project._

Earlier this year I moved to a new note taking solution, [Trilium Notes](https://github.com/zadam/trilium). After a lengthy discovery process including learning about the world of networked brain apps and Zettlekasten, I settled on Trilium for several reasons: it's open source, self-hosted with a browser interface, has extensive markup functionality including the ability to export to Markdown (this article was written in Trilium), and is **extensible via a note type called a ‘**[**script note**](https://github.com/zadam/trilium/wiki/Scripts)**’.** This allows you to extend functionality by simply writing JavaScript to run on either the frontend (chromium via electron) or backend (node.js) of the application. There are honestly so few downsides to this application, but if I had to list some it would be a slightly ugly interface (fixable via CSS), and no mobile app.

Previously, with Google Keep, I used to write myself notes, and then add alerts to be reminded of their content when I needed it. This worked as a reminder system for me well. So, I've set out to replicate this in Trilium, via the aforementioned script notes, and a self-hosted push notification service called [Gotify](https://gotify.net/). Let's get started.

Architecture
------------

Trilium features a number of proof-of-concept implementations of script notes. One of these is a task manager, that allows you to group tasks in to categories, provide a to-do date, and a done date. These are stored in attributes called `todoDate` and `doneDate`, respectively.

I have been using this task manager for a while now, so I'd like to add notification functionality to it. The target functionality is that when a note's `todoDate` is the current date, I should get a push notification on my Android phone, with the title of the note as the notification content.

### Gotify

To achieve this, I need a way of facilitating the notification on a mobile device. As Trilium has no mobile app, we can't use direct notification integration. Instead, we can use a notification service, such as [Gotify](https://gotify.net/). Gotify is an incredibly simple websocket based notification server. It takes notification messages in from ‘apps’, and publishes all notifications received to ‘clients’. Gotfiy has a webclient and an Android app, which provides the Android notification integration I'm looking for.

Gotify was trivial to set-up. I used [docker-compose](https://docs.docker.com/compose/) to have a container up and running in about 10 minutes. My config ended up looking like this:

    version: "3"
    
    services:
      gotify:
        image: gotify/server
        restart: "unless-stopped"
        ports:
          - 8085:80
        environment:
          - TZ="Pacific/<redacted>"
        volumes:
          - "./gotify_data:/app/data"
          - "./config.yml:/etc/gotify/config.yml"

I've added an additional volume where I've overridden the default config.yml. I'll explain why later, but take note it was important for my implementation.

Some additional configuration was required to allow websocket requests through my reverse proxy server, which also applies HTTPS encapsulation. Examples on how to do this can be found on the [Gotify website for all major reverse proxies](https://gotify.net/docs/nginx).

So now we have Gotify up and running, let's test it. An easy first test can be achieved with curl:

    josh@SATURNV-NT:~> curl "https://push.576i.nz/message?token=AHu8O88QRGLNaG0" -F "title=test 123" -F "message=holy guac it works" -F "priority=5"

And the result:

    {"id":2,"appid":1,"message":"holy guac it works","title":"test 123","priority":5,"date":"2021-10-30T05:32:09.401684375Z"}

We can see on the webclient they've come through:

{{< with .Resources.GetMatch "gotify-testing.png" >}}
<img src="{{ .RelPermalink }}"></img>
{{ end }}

![Testing](/images/gotify-testing.png)

Okay. So we have a working notification server. Now how do we implement this in Trilium?

### Websockets and JavaScript

Converting our simple curl one-liner to JavaScript proved more… difficult. By the end of the process, I knew substantially more about the differences between web-browser JS, node.js JS, and how to structure a websocket request than I bargained for.

Trilium, as a modern Electron webapp, is made up of two separate halves, the frontend and backend. The frontend runs in chromium (the embedded browser in Electron), and as such you can expect a typical web-browser based JS setup. The backend, in this case, is based on node.js. node.js doesn't implement a number of typical browser-based JS libraries.

Naively, I started by writing the script externally in VSCode, testing in browser. Gotify's example script uses Axios, a library used for HTTP operations. I had difficulty getting it to work successfully, so I consulted the experts, who advised me to use [fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API). Fetch is a native browser-based HTTP library, and with some tinkering, we got the following going:

    const url = "https://push.576i.nz/message?token=AHu8O88QRGLNaG0";
    
    let bodyFormData = new FormData();
    bodyFormData.append('title', 'hello from trilium');
    bodyFormData.append('message', 'omg it actually works from my note app');
    bodyFormData.append('priority', '5');
    
    fetch(url, {
      method: "POST",
    
      headers: {
        "Content-Type": "Content-Type: multipart/form-data; charset=utf-8; boundary='';"
      },
      body: bodyFormData
    });

To get to this point, we had to wrestle with several speed bumps. You may recall I had overridden the default config for Gotify in my docker-compose.yml.

The reason for this is so that I could add a CORS header to all requests. The scope of this article doesn't expand to CORS. but essentially the main thing that you need to know is that it's a security check based on domains. Gotify's Docker image has a number of environment variables that allow you to configure these headers, but the '\*' required for any origin is incompatible with the yaml globbing in docker-compose. I circumvented this by simply storing the default config locally and then importing it into Docker as a volume, which allows me to specify this line in the configuration:

    responseheaders: # response headers are added to every response (default: none)
    #    X-Custom-Header: "custom value"
        Access-Control-Allow-Origin: "*"

This resolves the CORS issues.

### Trilium

So, we have a working fetch-based script. I pulled this into Trilium, and loaded it up as a frontend script. Upon testing, it worked! So, how do I populate this push function with data from my todo notes? Turns out the ability to get attributes of notes (what the `todoDate` label is) can only be done from the backend. Okay, no problem, I can pass from the frontend to the backend to get the data I need, and do the rest of the logic in the frontend.

The next snag is that Trilium's in-built triggers that will fire my script for me, can only be run from the backend. You can pass from the frontend to the backend, but not the inverse of this. What this means is that if your script starts in the backend, it cannot be passed to the frontend at any point. Okay, no problem. I can just put fetch function into the backend script and run everything entirely in the backend.

Unfortunately, fetch is not implemented natively in node.js. This can be installed with npm, but with Trilium, I can't add packages to the backend. So I'm stuck.

Luckily, the Trilium backend implements Axios! This is only slightly annoying, because I could've theoretically skipped the fetch stage entirely. However, it wasn't a waste as it gave good opportunity to debug the CORS headers.

I rewrote the fetch request in Axois, based on the supplied example from Gotify:

    const axios = require("axios");
        
        const url = "https://push.576i.nz/message?token=AHu8O88QRGLNaG0";
        let bodyFormData = {
          title: a,
          message: b,
          priority: c
        };
    
        axios({
          method: "post",
          headers: {
            "Content-Type": "application/json"
          },
          url: url,
          data: bodyFormData
        }

This was then added to a larger script that call's Trilium's API, following simple logic:

1.  Get today's date
2.  Convert this to ISO format and truncate it to just yyyy-mm-dd
3.  Get all notes with this value in the `todoDate` field as an array
4.  Iterate through this array and send a push request for each one

So the final complete Trilium script is:

    // Get current date and format it using the ISO standard
    let today = new Date();
    let isotoday = today.toISOString();
    
    // Get all notes with the label 'todoDate' and today's date as the value, truncating the time from the ISO date
    let todoToday = api.getNotesWithLabel("todoDate", isotoday.substring(0, 10))
    
    // Iterate through the array and send a push with the title as the message body
    for (let i = 0; i < todoToday.length; i++) {
      sendPush('Trilium Reminder', todoToday[i].title, 5)
    }
    
    function sendPush(a, b, c) {
        const axios = require("axios");
        
        const url = "https://push.576i.nz/message?token=AHu8O88QRGLNaG0";
        let bodyFormData = {
          title: a,
          message: b,
          priority: c
        };
    
        axios({
          method: "post",
          headers: {
            "Content-Type": "application/json"
          },
          url: url,
          data: bodyFormData
        })
    };

As you can see, this works beautifully:

![Working](/images/gotify-working.png)

And on mobile:

![Phone](/images/gotify-phonenoti.jpg)

_If you had any comments or feedback, drop me a line at_ [_josh@576i.nz_](#root/lqptHG6sN2rj) _and I'll include interesting discussions on this page._