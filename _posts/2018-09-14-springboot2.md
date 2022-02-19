---
layout: post
title: '스프링 부트에서 MyBatis 활용하기'
author : 효준
date: 2018-09-14 11:36
tags: [spring]
image: /files/covers/blog.jpg
---

# 스프링 부트에서 MyBatis 활용하기
    스프링 부트에서는 기본적으로 MyBatis보다 JPA사용을 권장하는 것 같다.
    하지만 MyBatis를 활용하는 법도 알기위해 시도해보았다.
    
    --application.properties

    spring.datasource.driver-class-name=com.mysql.jdbc.Driver
    spring.datasource.url=jdbc:mysql://**:3306/**
    spring.datasource.username=**
    spring.datasource.password=**

    나는 mysql을 사용하므로 위와 같이 셋팅을 한다.
    
    --build.gradle
    
    dependencies {
        compile('org.springframework.boot:spring-boot-starter-jdbc')
        compile('org.springframework.boot:spring-boot-starter-web')
        compile("org.mybatis:mybatis-spring:1.3.2")
        compile('org.mybatis.spring.boot:mybatis-spring-boot-starter:1.3.2')
        runtime('org.springframework.boot:spring-boot-devtools')
        runtime('mysql:mysql-connector-java')
        testCompile('org.springframework.boot:spring-boot-starter-test')
        // https://mvnrepository.com/artifact/com.google.code.gson/gson
        compile group: 'com.google.code.gson', name: 'gson', version: '2.8.0'
    
    }
    
    나의 dependency 목록이다.
    
    
    -- Controller
    
    package hj.bus.demoapi.controller;
    
    import hj.bus.demoapi.mapper.BusMapper;
    import hj.bus.demoapi.util.JsonUtil;
    import hj.bus.demoapi.vo.BusStationVO;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.RestController;
    
    import javax.servlet.http.HttpServletRequest;
    import java.net.URLDecoder;
    import java.util.List;
    
    @RestController
    public class BusApiController {
    
        @Autowired
        private BusMapper busMapper;
    
        @RequestMapping(value = "/busApi", produces="text/plain;charset=UTF-8")
        public String init(HttpServletRequest res) throws Exception {
            String busNo = res.getParameter("busNo");
            String param = URLDecoder.decode(busNo,"UTF-8");
    
            List<BusStationVO> getList = busMapper.getList(param);
            System.out.println(getList);
    
            return JsonUtil.ListToJson(getList).toString();
        }
    
    
    }

    패키지를 잘 보아야 한다.
    
    
    
    package hj.bus.demoapi.mapper;
    
    import hj.bus.demoapi.vo.BusStationVO;
    import org.apache.ibatis.annotations.Mapper;
    import org.springframework.stereotype.Repository;
    
    import java.util.List;
    
    @Repository
    public interface BusMapper {
        public List<BusStationVO> getList(String busRouteNm) throws Exception;
    }


    Mapper 인터페이스이다.
    
    --AppApplication
    
    package hj.bus.demoapi;
    
    import org.apache.ibatis.session.SqlSessionFactory;
    import org.mybatis.spring.SqlSessionFactoryBean;
    import org.mybatis.spring.annotation.MapperScan;
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;
    import org.springframework.context.annotation.Bean;
    import org.springframework.core.io.Resource;
    import org.springframework.core.io.support.PathMatchingResourcePatternResolver;
    
    import javax.sql.DataSource;
    
    @SpringBootApplication
    @MapperScan(value={"hj.bus.demoapi.mapper"})
    public class DemoapiApplication {
    
        public static void main(String[] args) {
            SpringApplication.run(DemoapiApplication.class, args);
        }
    
        @Bean
        public SqlSessionFactory sqlSessionFactory(DataSource dataSource) throws Exception{
            SqlSessionFactoryBean sessionFactory = new SqlSessionFactoryBean();
            sessionFactory.setDataSource(dataSource);
    
            Resource[] res = new PathMatchingResourcePatternResolver().getResources("classpath:mappers/*Mapper.xml");
            sessionFactory.setMapperLocations(res);
    
            return sessionFactory.getObject();
        }
    
    }
    
    우선 MapperScan을 통해 mapper가 있는 곳을 스캔하고
    
    아래에 보면 @Bean을 통해 매칭을 해주는 SqlSessionFactory를 생성한다.
    매퍼와 매핑된 xml파일은 resources 밑에 mappers/*Mapper.xml 의 형태이다.
    
    --BusMapper.xml
    
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE mapper
            PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
            "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
    
    <mapper namespace="hj.bus.demoapi.mapper.BusMapper">
    
        <select id="getList" parameterType="String" resultType="hj.bus.demoapi.vo.BusStationVO">
            select * from db_busStation
             where busRouteNm = #{_parameter}
        </select>
    
    </mapper>
    
    매퍼의 네임스페이스를 맞춰주고 사용하던것처럼 하면 된다~