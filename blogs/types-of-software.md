---
title: Types of Websites
link: types-of-websites
published_date: 2024-07-23T00:00:00.000Z
meta_description: 'Oh, you like the internet? Name every website'
make_discoverable: false
---
Table of contents

<!-- toc -->

- [The Three Types](#the-three-types)
  * [Content](#content)
  * [Transactional](#transactional)
  * [Realtime](#realtime)
- [On Interactivity](#on-interactivity)
  * [Most Interactivity Isn't Complex](#most-interactivity-isnt-complex)
- [More Examples](#more-examples)
<br></br>

<!-- tocstop -->

## The Three Types

I find most websites I use can be broadly classified into one of three categories.

### Content

The primary type of websites I use are **content-driven sites**. These sites focus almost exclusively on _showing_ me information. My canonical example of such a site is [Wikipedia](https://www.wikipedia.org/): the shining epitome of hypermedia (the "H" in HTML) and web standards. But a lot more sites are wiki-like than you'd initially think.

Web blogs were also traditionally static HTML pages hosted on some public domain. Today they might be built with SPA frameworks to offer more interactivity, but at their core blogs are just files of text content hosted on a web server. Many websites generalize this concept of “just a file” to apply to other media types. You could argue that YouTube is a blog that hosts video files instead of HTML.

Personalized social media like Facebook often functions just like a static blog to end users. While Facebook needs to provide extra server capabilities to authenticate users, personalize feeds, handle post submissions, etc.; once all that is done, Facebook could just serve a static HTML page for every post and achieve most of its core functionality.

### Transactional

Another category of website I use regularly are those that provide remote **data transactions**. These sites primarily enable me to manage structured data through a series of discrete transactions. My most used example is Google Calendar, though I'm actually now using Proton Calendar in a fruitless attempt to [degoogle-ify](https://en.wikipedia.org/wiki/DeGoogle) my life. Calendaring sites let you manage and view your schedule data through discrete interactions like adding a new event or viewing your upcoming week.

By "transactional" and "discrete interactions" I specifically mean that these types of websites operate on a request-and-response model. You fill out a form to make a new calendar event and press "Create" to send a request for Google to make the event. Google servers make the event and send a response telling you the event was made.

I'd say most business SaaS websites are transactional in nature. Tools like Jira and Asana provide discrete mechanisms for updating task data; TODO: another example site. I think of transaction-focused websites as being nice UI wrappers around spreadsheets. Most of what I could accomplish with Jira or Calendars I could do with a spreadsheet, though it might be a less "magical" experience.

### Realtime

TODO: spreadsheets, games, creative tools, etc

## On Interactivity

Notice that, when compared to Transactional and Realtime websites, Content sites are generally not very interactive. Similarly, Realtime sites make Transactional sites look static in comparison. Yet all three types do share one type of user interaction: the "flourish." Modern browsers enable website creators to imbue their HTML and CSS with animation and style transitions to give their designs some "juice." Think of mouse trail effects, drop shadow animations on button clicks, or robust data visualizations.

TODO: something about the spectrum from flourishes to realtime

In my experience using the web for the last 20 years, I almost exclusively interact with websites through flourishes and transactions. In fact, when websites try to force realtime interactivity onto me, like live Google Maps route updates or countdown timers to ticket sales, I actively _distrust_ the information I'm seeing. If a website's natural mode of interaction is transactional then I naturally think of data as being stale if it was not the direct response of a recent transaction.

### Most Interactivity Isn't Complex

TODO: something-something the web naturally lends itself to transactions, so we should do that well and without big JS bloat

## More Examples

TODO: triangle spectrum diagram
