---
layout:     post
title:      "mike-coconut"
subtitle:   "Offline apps"
date:       2015-05-15 15:00:00
author:     "rocknrollers"
header-img: "img/post-bg-07.jpg"
photo-source: "https://www.flickr.com/photos/pierre_pouliquin/143762891"

---

Thanks to Mike for providing his notes and slides. Mike walked us through some work he's done lately with offline apps. Really great stuff, Mike!


# Agenda

  * Gooseberry
  * Coconut
  * Offline Apps
  * Blockspring
  * Dropbox SDK

---

# Gooseberry

* Collect Data via Interactive SMS
* Ruby Sinatra + CouchDB
* Zanzibar: Health Clinics report new Malaria Cases
* Kenya: Registers attendees eventually sends them money

---

<img width="200%" src='http://dl.dropbox.com/u/18491046/Selection_001.png'>

---

<img width='200%' src='http://dl.dropbox.com/u/18491046/Selection_003.png'>

---

# Coconut

_Beyond Collecting Data_

---

# Architecture

* HTML5 Cloud Backed Offline Application
* CouchDB in the Cloud (HTTP Server, NOSQL Database)
* PouchDB (used to be CouchDB) on the client

---

# Normal HTML 5 App

* Backbone for MVC
* JQuery Mobile for layout
* Data Collection in the field
  * PouchDB for local storage and cloud syncing
* Data Reporting from the cloud
  * CouchDB (sometimes a bit of Ruby too)

---

# Coconut Data Structures

* All Data are JSON documents
* Data are mostly Questions or Results

---

# Questions

{% highlight json %}

"_id": "Household",
  "questions": [
      {
          "id": "531",
          "label": "Malaria Case ID",
          "type": "text"
      },{
          "id": "401",
          "label": "Head of Household Name",
          "type": "autocomplete from previous entries"
      }, {
          "autocomplete-options": "window.ShehiaOptions = GeoHierarchy.allUniqueShehiaNames()",
          "id": "398",
          "label": "Shehia",
          "repeatable": "false",
          "type": "autocomplete from code",
          "validation": "($('#398').val('');return 'Shehia ' + value + ' is not valid. Please try again.' ) unless _.contains(window.ShehiaOptions, value)"
      }
...
{% endhighlight %}

---

# Results

{% highlight json %}

{
  "_id": "a50615950003474e6f749b3b2000049b",
  "_rev": "19-7a75c672aeccc1a13b289586bf1e557b",
  "createdAt": "2012-01-03 23:59:46",
  "lastModifiedAt": "2012-01-04T00:04:26+00:00",
  "question": "Facility",
  "MalariaCaseID": "1016807",
  "FacilityName": "KIJINI",
  "Shehia": "KIJINI 1",
  "user": "0773922266",
  "collection": {
  "_id": "a50615950003474e6f749b3b2000049b",
  "_rev": "19-7a75c672aeccc1a13b289586bf1e557b",
  "createdAt": "2012-01-03 23:59:46",
  "lastModifiedAt": "2012-01-04T00:04:26+00:00",
  "question": "Facility",
  "MalariaCaseID": "106807",
  "FacilityName": "KIJINI",
  "Shehia": "KIJINI 1",
  "user": "0773922266",
  "collection": "result",
  "DateofPositiveResults": "2015-03-26",
  "ParasiteSpecies": "PF",
  "ReferenceinOPDRegister": "9999",
  "FirstName": "kombo",
  "MiddleName": "faki",
  "LastName": "mcha",
  "Age": "25",
  "Village": "zingani",
  "ShehaMjumbe": "hassamn mcha",
  "HeadofHouseholdName": "kombo faki mcha",
  "ContactMobilepatientrelative": "9999",
  "TreatmentGiven": "act",
  "IfYESlistALLplacestravelled": null,
  "CaseIDforotherhouseholdmemberthattestedpositiveatahealthfacility": null,
  "CommentRemarks": null,
  "complete": "true",
  "savedBy": "0773922266",
  "AgeinMonthsorYears": "Years",
  "Sex": "Male",
  "TravelledOvernightinpastmonth": "No",
  "Hassomeonefromthesamehouseholdrecentlytestedpositiveatahealthfacility": "No"
}

{% endhighlight %}

# Offline Apps

* Add a file called manifest.appcache at the root of your web application
  * A request for any of the URLS will start by trying to get the manifest.appcache
  * FAILS =>  cached version used.
  * SUCCEEDS and manifest.appcache file SAME => cached version  used.
  * SUCCEEDS and manifest.appcache file DIFFERENT => everything is reloaded

---

Another member of the group piped up. "Hi, I'm ApplicationCache," he said, as he reached over to shake Dev's hand. "I turn your offline experience from sucks-ass, to success. Just one extra file, and bosh! It works. No fuss, no 'scripting' necessary." Yes, he did finger-quotes while saying 'scripting.' I'm gritting my teeth at this point, because I know he's greatly exaggerating his abilities and the others don't see it. However, if I call "bullshit" on it I'll seem like the jerk.

I felt bad for not doing anything on that completely real evening. It's painful to see articles that praise ApplicationCache's ease of use, written by people who've clearly only met him in passing. I must set the record straight: I'm here to tell you ApplicationCache is a douchebag.

Now, I don't mean he's useless or should be avoided, you just have to be very careful when and how you work with him. If you get it wrong, the douchebaggery oozes through onto the end user. By reading through my own painful experiences with AppCache, you'll know what to expect from AppCache and how to deal with it.

---

# [http://alistapart.com/article/application-cache-is-a-douchebag](http://alistapart.com/article/application-cache-is-a-douchebag)

  ![](http://dl.dropbox.com/u/18491046/Selection_004.png)

except this:

# [https://github.com/gr2m/appcache-nanny](https://github.com/gr2m/appcache-nanny)

---

{% highlight bash %}

CACHE MANIFEST
/
/js/app.min.js
/js/libs.min.js
/css/style.min.css
/index.html

NETWORK:
*
http://*

FALLBACK:
/ /
# VERSION 5asd1s

{% endhighlight %}

---


{% highlight html %}

<html>
  <meta charset='utf-8'>
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="mobile-web-app-capable" content="yes">
  <head>
    <title>Coconut</title>

{% endhighlight %}


---

# Vision

  * Deployment Script with Docker
  * Merge question format of Gooseberry/Coconut
  * Enable data capture for any Coconut form via SMS or Tablet
  * UI Refactor
    * Fork server side reporting and client side data collection UI
    * Strip out JQuery Mobile
    * Use a bootstrap template
  * Consider Elastic Search and Kibana as new basis for reporting

---

# Blockspring

[https://www.youtube.com/watch?v=UD3a_Eqj5ew](https://www.youtube.com/watch?v=UD3a_Eqj5ew)

---

# Dropbox SDK

{% highlight ruby %}
access_token = "bHKasfYaEVYvAAANotRealTokenSoDontTryItOKkOBOeB1J3A5D2I0tQsIyg"
dropbox_client = DropboxClient.new access_token

csvs_required.each do |csv_required|
  question_set = QuestionSet.new(csv_required["question_set"])
  filename = csv_required.values.join("--")+".csv"
  File.open(filename, "w") do |file|
    file.write question_set.csv_header + "\n"
    file.write question_set.rows(csv_required["start_date"],csv_required["end_date"])
  end
  dropbox_client.put_file filename,open(filename),true
end

{% endhighlight %}

---