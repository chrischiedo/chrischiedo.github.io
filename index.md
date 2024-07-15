---
layout: home
redirect_from:
  - /project/
  - /junk/
---

# Chrispine Chiedo

Hey there! My name is Chrispine. Welcome to my internet home.

### A little bit about me...

I am a software engineer and technical trainer. I am mainly interested in distributed systems and programming languages (from a design and implementation standpoint). Over the years, I've worked with Java, Python, and Rust.

For more about me, please check out the [about]({% link about.md %}) page.


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
