---
layout: default
---

# About me

I have been working directly with software development for five years using Java and Kotlin language, open source frameworks and tools, with excellent experience in the execution of projects, from obtaining requirements until the implementation in production. I participated in the most varied sizes of projects, from small projects for internal uses to large and global projects, involving clients like Google and Edenred.

Solid experience in developing reactive microservices and cloud applications using Java and Kotlin language with the frameworks and concepts such as: Spring Stack, Vert.x, gRPC, Openshift, Kubernetes, Docker, Prometheus, AWS, JMS, AMQP, ORM, REST, DDD and CDI. 

# Posts

<div>
  {% for post in site.posts %}
      <a href="{{ post.url }}">
        <div>
           <h2>{{ post.title }}</h2>
           <p style="color: #666;" >{{ post.summary }}</p>
           <p>{{ post.tags }}</p>
        </div>
      </a>
  {% endfor %}
</div>