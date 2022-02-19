---
layout: post
title: '인텔리j 스프링부트 mustache 엔진'
author : 효준
date: 2018-06-21 22:03
tags: [egov]
image: /files/covers/blog.jpg
---

# 인텔리j 에서 템플릿엔진으로 Mustache 사용하기

인텔리j에서 템플릿엔진으로 Mustache 설정 후에 컨트롤러에서 templates 폴더 안에 있는<br/>
html을 반환 못해주는 경우가 있다.

```
import org.springframework.boot.web.servlet.view.MustacheViewResolver;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.ViewResolverRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebConfiguration implements WebMvcConfigurer {
    @Override
    public void configureViewResolvers(ViewResolverRegistry registry) {
        MustacheViewResolver resolver = new MustacheViewResolver();

        resolver.setCharset("UTF-8");
        resolver.setContentType("text/html;charset=UTF-8");
        resolver.setPrefix("classpath:/templates/");
        resolver.setSuffix(".html");

        registry.viewResolver(resolver);
    }
}
```
클래스를 하나 만들어서 이렇게 MustacheViewResolver를 만들어준다. 그러면 될것이다.