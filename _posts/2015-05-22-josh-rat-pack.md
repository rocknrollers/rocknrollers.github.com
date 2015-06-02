---
layout:     post
title:      "josh-sinatra-i18n-daybreak"
subtitle:   ""
date:       2015-05-22 15:00:00
author:     "rocknrollers"
header-img: "img/post-bg-08.png"
photo-source: "https://www.flickr.com/photos/oddsock/109594846"
---

This will mark our 10th session. Thank you rocknrollers for always pitching in and rocking out.

In other news, Adam is being awarded with a hall of fame badge. He is being promoted to Mick Jagger. Remember, with great power comes great responsibility.

Really quickly

* [18next](http://i18next.com/) - simple javascript library for translating your ui. Very flexible and very minimalist. When you look at the feature list and examples you'll feel very confident that it will handle whatever language you can throw at it.

* [sinatra](http://sinatrarb.com) - micro-framework for Ruby.

* [daybreak](http://propublica.github.io/daybreak/) - impressive little keystore database.

For this quick Sinatra demo we created a backend to handle bit.ly type requests. In English, if a submitted url is known, provide the token, shortened url, othewise, save the url and provide a random token. If a get request is made to a known token, then redirect. Easy peasy lemon squeezy.

{% highlight ruby %}

require 'daybreak'
require 'sinatra/base'

$db = Daybreak::DB.new "url.db"

def random_string 
  (('a'..'z').to_a + ('A'..'Z').to_a).shuffle[0,4].join
end

class UrlApp < Sinatra::Base

  post '/shorten' do
    url = params[:url]
    token = random_string
    
    $db.set! token, url
    
    token
  end

  get '/:token' do
    redirect $db[params[:token]]
  end

end

{% endhighlight %}