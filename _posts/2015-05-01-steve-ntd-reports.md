---
layout:     post
title:      "steve-ntd-reports"
subtitle:   "Lord of the pings: MySQL, security and a helpful wizard"
date:       2015-05-01 15:00:00
author:     "rocknrollers"
header-img: "img/post-bg-05.jpg"
photo-source: "https://www.flickr.com/photos/anirudhkoul/3804551632"
---

This week Steve kindly walked us through the work he’s been doing on the NTD database system. His component is a rails application that creates reports for a tricky database situation.

While he was explaining I had to google the following things and learned quite a bit.

[monit](http://mmonit.com/monit/)
Great tool for monitoring processes.

[Pivot tables](http://en.wikipedia.org/wiki/Pivot_table)
Way to aggregate data by columns.

[jQuery Steps](http://www.jquery-steps.com/)
Plugin for making wizards.

Steve also talked about the difficulty of having a main application (drupal based) communicate with a secondary application (in rails) and juggle the authentication between the two of them.

The solution goes a little something like this

1. An authenticated user on drupal has a link that contains an encrypted string, a “multipass token” at the end. The password to decrypt the token lives on the drupal server and on the rails server, so even though the user gets a link that looks like newserver.com/login/adASDadaDAsdas no one can tell what’s in the data. The string actually contains all the user’s information that the rails app needs to do it’s job. 

2. Next the rails server gets the request with the token and attempts to decrypt it. If it successfully decrypts, and decrypts into a valid predetermined structure, then it’s considered a valid user created by the drupal server. Voila!

The solution here was surprisingly simple and breathtakingly pragmatic, like most good solutions, I find. Steve says he learned the idea from EasyBI, another tool we’ve used in the past.

Please let me know if I left out anything cool.

Thanks for reading.