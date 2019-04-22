---
layout: page
title: About
meta: "Some info about Galuh, what she does currently and previously, as well as the talks she has given before."
permalink: /about/
---

Hi there! My name is **Galuh**.

Three of my favorite things are **data, tech, & design**. My passion is making data science and computer science more accessible to people.

Right now I:
- Am working as a data engineer at GOJEK
- Co-hosting [Kartini Teknologi](https://kartiniteknologi.id), a podcast featuring Indonesian women in tech and [Satu Halaman Lagi](https://instagram.com/satuhalamanlagi), a podcast where my friends & I talk about the book we're reading

Sometimes I:

- Make [fun side projects](/projects). I love mixing web development + data!
- Share things I've learned by [speaking at meetups and conferences](/about#publication-and-talks)
- Translate <a href="https://github.com/sdras/array-explorer">open source</a> <a href="https://github.com/sdras/object-explorer">projects</a> and <a href="https://developer.mozilla.org/en-US/profiles/galuhsahid">documentation</a> from English to Bahasa Indonesia
- [Sketch places](/sketches) and [read books](http://bookshelf.galuh.me)
- [Write random notes](http://notes.galuh.me)

Previously I:

- Co-organized the first [Global Diversity CFP Day](https://www.globaldiversitycfpday.com/events/90) Jakarta. Here's a [podcast](https://devmuslim.id/post/088-global-diversity-cfp-day-jakarta-2019/) that discusses it
- Studied computer science at Universitas Indonesia
- Co-organized [CompFest](http://compfest.web.id), the biggest student-run IT event in Indonesia. Here's [my write-up of what we do](https://medium.com/tales-of-stepitup/stepitup-the-beginning-514b614064a3), [a bunch of lessons learned](/categories/#stepitup), and [some](https://komunika.tempo.co/read/news/2016/09/26/273807423/magz.tempo.co?lipi=urn%3Ali%3Apage%3Ad_flagship3_profile_view_base_treasury%3Bjv6f0XbxS6CantEOZLMWqw%3D%3D#.WhY0iUuLn6Y) [publication](https://id.techinasia.com/ide-iot-yang-terpilih-di-iot-academy-compest-8?lipi=urn%3Ali%3Apage%3Ad_flagship3_profile_view_base_treasury%3B%2BZlzFG7iT6KpCYpz3S7plg%3D%3D) covering our work
- Participated in the [Increasing Rust's Reach](http://reach.rust-lang.org) program. I'm working on [Diesel](http://diesel.rs), a safe, extensible ORM and Query Builder for Rust
- Did various freelance graphic design, web design, and web development projects
- Made illustrations for fun

I can be reached through galuh.tunggadewi[at]gmail.com. I'm also on [Twitter](http://twitter.com/galuhsahid), [Medium](http://medium.com/@galuhsahid), and
[GitHub](http://github.com). 

## Publication and Talks
<ul class="list">
{% for item in site.data.talks-publication %}

    <li class="list__item">
        <div class="list__title">{{ item.title }}</div>
        <div class="list__subtitle"><a href="{{ item.website }}">{{ item.where }}</a></div>
        <div class="list__details">
            <div class="list__sub-item"><span class="list__location">{{ item.location }}</span></div>
{% if item.what == "talk" %}
    <div class="list__sub-item"><span class="list__code"><a href="{{ item.code }}">Code</a></span></div>
    <div class="list__sub-item"><span class="list__slides"><a href="{{ item.slides }}">Slides</a></span></div>
{% endif %}

{% if item.what == "paper" %}
    <div class="list__sub-item"><span class="list__authors">{{ item.author }}</span></div>
    <div class="list__sub-item"><span class="list__paper"><a href="{{ item.paper }}">Paper</a></span></div>
{% endif %}
        </div>
    </li>
{% endfor %}

</ul>

## Colophon
This site is built on [Jekyll](http://jekyllrb.com/) and hosted on [GitHub Pages](https://pages.github.com/). To write the CSS, I used [Sass](http://sass-lang.com/) with the help of [sass-boilerplate](https://github.com/HugoGiraudel/sass-boilerplate). To make development easier, I used [jekyll-gulp-sass-browser-sync](https://github.com/shakyShane/jekyll-gulp-sass-browser-sync) starter project which includes a full setup for Jekyll, GulpJS, SASS, AutoPrefixer & BrowserSync. Fonts used are [Sura](https://fonts.googleapis.com/css?family=Sura) and [FontAwesome](https://fontawesome.com/). [Here is the source code](http://github.com/galuhsahid/galuhsahid.github.io) of this site.

