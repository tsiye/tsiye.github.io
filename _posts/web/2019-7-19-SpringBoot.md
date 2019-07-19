---
layout: post
title: SpringBoot
category: 技术向
tags: [SpringBoot,Swagger,Postgres,Mybatis]
description: 实习的一切SpringBoot相关
---

## Swagger
### 添加依赖
```xml
<dependency><!--添加Swagger依赖 -->
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.7.0</version>
</dependency>
<dependency><!--添加Swagger-UI依赖 -->
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.7.0</version>
</dependency>
```
### 配置
```java
@Configuration
@EnableSwagger2
public class Swagger2 {
    @Bean
    public Docket buildApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.example.demo.controller"))
                .paths(PathSelectors.any())
                .build()
                .pathMapping("/")
                .apiInfo(apiInfo());
    }

    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("SpringBoot Swagger2 Restful Api")
                .description("SpringBoot 使用 Swagger2 生成 Restful Api .  ")
                .version("1.0")
                .contact(new Contact(
                        "tsiye",
                        "https://tsiye.github.io",
                        "chiyuanye@chiyuanye.com"))
                .build();
    }
}

```
### model加上注释
```java
@ApiModelProperty(hidden = true)
private String id;

@ApiModelProperty(value = "用户名")
private String  username;

@ApiModelProperty(value = "密码")
private String password;
```

### controller层加注释
`@Api`和`@ApiOperation`添加对接口和接口函数的描述
swagger会根据`@PathVariable`自动生成注释
`@ApiImplicitParam`描述方法的参数

```java
@RestController
@RequestMapping(value = "/user", produces = APPLICATION_JSON_VALUE) //配置返回值 application/json
@Api(description = "用户管理")
public class HelloController {
    ArrayList<User> users = new ArrayList<>();


    @ApiOperation(value = "获取用户列表", notes = "获取所有用户信息")
    @RequestMapping(value = {""}, method = RequestMethod.GET)
    public List<User> hello() {
        users.add(new User("逻辑", "luoji"));
        users.add(new User("叶文杰", "yewenjie"));
        return users;
    }

    @ApiImplicitParam(name = "user", value = "用户详细实体user", required = true, dataType = "User")
    @ApiOperation(value = "创建用户", notes = "根据User对象创建用户")
    @RequestMapping(value = "/create", method = RequestMethod.POST)
    public User postUser(User user) {
        return user;
    }

    @ApiOperation(value = "获取用户详细信息", notes = "根据url的id来获取用户详细信息")
    @RequestMapping(value = "getUser/{id}", method = RequestMethod.GET)
    public User getUser(
            @PathVariable(value = "id") String id) {

        return new User(id, "itguang", "123456");
    }
}
```

## reference
1. [Swagger2教程](https://www.cnblogs.com/Shadowplay/p/10606945.html)
2. [一篇文章带你搞懂 SpringBoot与Swagger整合](https://blog.csdn.net/itguangit/article/details/78978296)
