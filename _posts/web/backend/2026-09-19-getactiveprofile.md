---
title: "Spring Boot - 활성화된 profile 조회"
date: 2026-09-19 15:15:00 +0900
categories: [Web, Backend]
tags: [spring, springboot, environment, profile]
---

예제 코드
```java
@Component
@Slf4j
public class ExampleComponent{

	@Autowired
	private Environment environment;
	
	public void test(){
	  if(Arrays.asList(environment.getActiveProfiles()).contains("local")){
		  log.info("local");
	  }
	}
}
```