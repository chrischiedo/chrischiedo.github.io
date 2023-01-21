---
layout: home
redirect_from:
  - /project/
  - /junk/
---

# Chrispine Chiedo

### About me

I am an Electrical/Computer Engineer by training. I have spent a good chunk of
my professional life working in the technical training space: as a software
development trainer, electrical engineering instructor, a technical curriculum
developer, and as a Head of Department (ICT and Engineering) in a technical
training institute.

Most recently, I worked as a Technical Writer and the "unofficial" team lead for the Nairobi [identity platform
documentation](https://learn.microsoft.com/en-us/azure/active-directory/develop/)
team at [Microsoft](https://www.microsoft.com/).

I am currently pursuing my Master’s degree in Distributed Computing Technology
at the [University of Nairobi](https://www.uonbi.ac.ke/), School of Computing
and Informatics.

For more about my professional experience, check my long-form [resume]({% link resume.md %}).

### Featured Posts

<ul>
  {% for post in site.posts %}
    {% if post.tags contains 'publish' %}
      <li><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endif %}
  {% endfor %}
</ul>

For all posts, visit [blog]({% link blog.md %}).

### Side Projects

For a sample of my side projects, visit [projects]({% link projects.md %}).

### Books

Here is a list of some of my favourite [books]({% link books.md %}).

### Getting in touch

[Email](mailto:chrispinechiedo@gmail.com){:target="_blank"} &mdash;
[LinkedIn](https://www.linkedin.com/in/chrispinechiedo){:target="_blank"} &mdash;
[GitHub](https://github.com/chrischiedo){:target="_blank"} &mdash;
[Twitter](https://twitter.com/chris_chiedo){:target="_blank"}
