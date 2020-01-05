---
layout: post
title: "Spring boot 2.2 and Prometheus Pushgateway with Micrometer"
---

# Spring boot 2.2 and Prometheus Pushgateway with Micrometer

Hi, how are you? I hope everything is fine.

Well, I had been looking for some tutorials to expose some metrics to my Prometheus, but the challenge is that these metrics are from a Batch Job.

We know that [Prometheus](https://prometheus.io/) is a mature project and solve this problem to us with the [Prometheus Pushgateway](https://github.com/prometheus/pushgateway) that exists to allow ephemeral and batch jobs to expose their metrics to Prometheus. Since these kinds of jobs may not exist long enough to be scraped, they can instead push their metrics to a Pushgateway. The Pushgateway then exposes these metrics to Prometheus.

Well, Prometheus solves the problem! So let us integrate with Prometheus Pushgateway using Spring Boot and Micrometer? Stop! What is Micrometer?

[Micrometer](https://micrometer.io/) is a simple facade over the instrumentation clients for the most popular monitoring systems, allowing you to instrument your JVM-based application code without vendor lock-in. Think SLF4J, but for metrics.

Now that we know all the concepts about Prometheus Pushgateway and Micrometer, it is time to integrate them using Spring Boot, so let us start?

The first thing is to create a Spring Boot Maven Project, the easiest way to do that is using the [Spring Initializr](https://start.spring.io), after it, we need to add some dependencies:

Spring Web, to expose a simple API:

~~~
<dependency>
 <groupId>org.springframework.boot</groupId>
 <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
~~~

Spring Actuator, to enable the use of Micrometer:

~~~
<dependency>
 <groupId>org.springframework.boot</groupId>
 <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
~~~

Micrometer, to expose metrics:

~~~
<dependency>
 <groupId>io.micrometer</groupId>
 <artifactId>micrometer-core</artifactId>
</dependency>
~~~

Micrometer Prometheus, to format the metrics into the prometheus format:

~~~
<dependency>
 <groupId>io.micrometer</groupId>
 <artifactId>micrometer-registry-prometheus</artifactId>
</dependency>
~~~

Prometheus Pushgateway Client, to send the metrics to the Prometheus Pushgateway:

~~~
<dependency>
 <groupId>io.prometheus</groupId>
 <artifactId>simpleclient_pushgateway</artifactId>
</dependency>
~~~

Well, the Spring Boot is configured, now we need to configure the Prometheus Pushgateway, to do that we are going to use Docker, so just run the command below:

~~~
docker run -d --net=host --name=prometheus-pushgateway prom/pushgateway:latest
~~~ 

Let us test the Prometheus Pushgateway opening the `http:\\localhost:9091` address and navigating to it.

Well, everything is configured, so it is time to code and add some metrics to test the integration.

Create a simple Controller, using the Spring's Annotations, such as `@RestController`, `@RequestMapping` and `@GetMapping`.

~~~
@RestController
@RequestMapping("/hello")
public class HelloController {

  private final MeterRegistry meterRegistry;

  public HelloController(MeterRegistry meterRegistry) {
    this.meterRegistry = meterRegistry;
  }

  @GetMapping
  public String get() {
    Counter counter = Counter.builder("hello-counter").register(meterRegistry);
    counter.increment();
    return "Hello, counter = " + counter.count();
  }

}
~~~

Starts the application `mvn spring-boot:run` and send some request to `http:\\localhost:8080\hello`.

Now open again the Prometheus Pushgateway (`http:\\localhost:9091`) and verify if the metrics `hello_counter_total` is there.

# References

https://micrometer.io

https://prometheus.io

https://prometheus.io/docs/practices/pushing

https://www.callicoder.com/spring-boot-actuator-metrics-monitoring-dashboard-prometheus-grafana

https://github.com/spring-projects/spring-boot/pull/14353

# Codebase

The full example is found here: https://github.com/larchanjo/poc-spring-prometheus-gateway

**Be Happy**