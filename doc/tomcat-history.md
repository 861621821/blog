## 让Tomcat支持history模式
##### 一、项目根目录下创建`WEB-INF`文件夹
##### 二、`WEB-INF`文件夹中创建`web.xml`文件
##### 三、编辑`web.xml`文件，输入以下代码
``` xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                      http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
  version="3.1"
  metadata-complete="true">

  <display-name>webapp</display-name>
  <description>
    webapp
  </description>
  <error-page>  
    <error-code>404</error-code>  
    <location>/</location>  
  </error-page>  
</web-app>
```
##### 四、重启tomcat服务器