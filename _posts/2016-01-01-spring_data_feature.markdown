---
layout: post
title:  "spring data 의 몇몇 기능"
---
[spring data]:(http://docs.spring.io/spring-data/commons/docs/current/reference/html/)  
  
1. Domain Class Converter  
				 
```{{.java}}
     
    @Controller  
    @RequestMapping("/users")  
    public class UserController {  
    
      @RequestMapping("/{id}")  
      public String showUserForm(@PathVariable("id") User user, Model model) {  
        model.addAttribute("user", user);  
        return "userForm";  
      }  
    }  
  
As you can see the method receives a User instance directly and no further lookup is necessary.   
The instance can be resolved by letting Spring MVC convert the path variable into the id type of the domain class  
first and eventually access the instance through calling findOne(…) on the repository   
instance registered for the domain type.  
  
...이런 게 될 줄이야..
  
[querydsl 번역]:(http://www.querydsl.com/static/querydsl/4.0.1/reference/ko-KR/html_single/)
