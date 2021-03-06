### Securing Camunda's REST API when using Spring Boot

This is an example project created with the Spring Boot Archetype to showcase how to secure the embedded REST API using Camunda's Authentication Filter.

More detailed information about how the filter works can be found in the [Docs](https://docs.camunda.org/manual/latest/reference/rest/overview/authentication/).

#### Show me the important parts

All you have to do is to add a `FilterRegistrationBean` to your Spring Boot app which registers a `ProcessEngineAuthenticationFilter`:

```java
package com.camunda.demo;

import org.camunda.bpm.engine.rest.security.auth.ProcessEngineAuthenticationFilter;
import org.camunda.bpm.engine.rest.security.auth.impl.HttpBasicAuthenticationProvider;
import org.springframework.boot.web.servlet.FilterRegistrationBean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class CamundaSecurityConfig
{

	@Bean
	public FilterRegistrationBean<ProcessEngineAuthenticationFilter> processEngineAuthenticationFilter()
	{
		FilterRegistrationBean<ProcessEngineAuthenticationFilter> registration = new FilterRegistrationBean<>();
		registration.setName("camunda-auth");
		registration.setFilter(this.getProcessEngineAuthenticationFilter());
		registration
			.addInitParameter("authentication-provider", HttpBasicAuthenticationProvider.class.getName());
		registration.addUrlPatterns("/engine-rest/*");
		return registration;
	}

	@Bean
	public ProcessEngineAuthenticationFilter getProcessEngineAuthenticationFilter()
	{
		return new ProcessEngineAuthenticationFilter();
	}
}

```

#### Run the project

You download the sample and run the app using `mvn spring-boot:run`. Then use a REST client of your choice and interact with the REST API after authenticating as the user `demo` with the password `demo`.