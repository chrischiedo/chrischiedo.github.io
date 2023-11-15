---
layout: home
redirect_from:
  - /project/
  - /junk/
---

# Chrispine Chiedo

Hey there! My name is Chrispine. Welcome to my internet home.

### About me

My background is in Electrical/Computer Engineering.

I have spent a good chunk of my professional life working in the technical training space: as a software
development trainer, electrical engineering instructor, a technical curriculum
developer, and a technical content developer. I have also served as a Head of Department (ICT and Engineering) in a technical training institute.

A couple of years ago I decided to begin transitioning into the software development space (a journey that I'm still on).

I've worked at [Microsoft](https://www.microsoft.com/) as a Technical Writer and the "unofficial" team lead for the Nairobi [identity platform
documentation](https://learn.microsoft.com/en-us/azure/active-directory/develop/).

I am currently pursuing a part-time Master’s degree in Distributed Computing Technology
at the [University of Nairobi](https://www.uonbi.ac.ke/), School of Computing
and Informatics.

My ultimate ambition is to end up as a technical programmer manager or a cloud solutions architect (I've already bagged one [AWS Cloud Certification](https://www.credly.com/badges/3f9765dc-6df8-402d-a5ee-b4d1595d5981/public_url)).

For more about my professional experience, please check out my [LinkedIn profile](https://www.linkedin.com/in/chrispinechiedo/).

### Featured Posts

<ul>
  {% for post in site.posts %}
    {% if post.tags contains 'publish' %}
      <li><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endif %}
  {% endfor %}
</ul>

For all posts, visit [blog]({% link blog.md %}).

### Open Source Contributions

For a sample of my open source contributions, visit [OSS contributions]({% link contribute.md %}).

### Books

Here is a list of some of my favourite [books]({% link books.md %}).

### Getting in touch

[Email](mailto:chrispinechiedo@gmail.com){:target="_blank"} &mdash;
[LinkedIn](https://www.linkedin.com/in/chrispinechiedo){:target="_blank"} &mdash;
[GitHub](https://github.com/chrischiedo){:target="_blank"} &mdash;
[Twitter](https://twitter.com/chris_chiedo){:target="_blank"}
