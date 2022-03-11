# Eclipse IDE for Java EE download + install
* lm01-01_CPS278_Eclipse_Tomcat_Mac-zhYf3U2wqXU.mp4 @ 00:10
* java --version
* need >= Java SE 11

# Tomcat setup
* lm01-01_CPS278_Eclipse_Tomcat_Mac-zhYf3U2wqXU.mp4 @ 08:54
* dowload Tomcat v9 (because it works with Spring 5, this may change with future versions)
* install locally in appropriate manner for your OS
* lm01-01_CPS278_Eclipse_Tomcat_Mac-zhYf3U2wqXU.mp4 @ 13:55
* start Tomcat server: find startup.sh where you installed Tomcat .. run it
* test that the server is running: http://localhost:8080/
* shutdown server: shutdown.sh
* go to Eclipse and do Tomcat setup there: servers tab

# Maven setup
* lm01-02_cps278_mavenDemo-mtDWY-NdsTk 02:47
* project manager that used POM (Project Object Model) approach
* pom.xml stores links to repos that your project depends on
* handles dependencies
* uses Maven Repositories
* Maven should be installed by default in Eclipse JEE
* check with: File > New > Maven Project
* if you don't have it: Help > Eclipse Marketplace > Find "m2eclipse"
* this should already be installed: "Eclipse m2e - Maven support in Eclipse IDE Latest" .. if it isn't, install it

# Maven project
* File > New > Maven Project
* make sure to uncheck "create a simple project (skip archetype selection)
* archetypes? they're project templates
* https://maven.apache.org/guides/introduction/introduction-to-archetypes.html
* https://maven.apache.org/archetypes/index.html
* we'll use "maven-archetype-quickstart"
* https://maven.apache.org/archetypes/maven-archetype-quickstart/
* Catalog: Internal .. then choose "maven-archetype-quickstart"
* Group Id: **edu.wccnet.ckingdon** (or something like this)
* Artifact Id: **mavenDemo** (for testing)

# Spring plugin setup
* lm01-04_CPS278_SpringPlugin--LxcUO0q7Zk.mp4
* in Eclipse > Help > Eclipse Marketplace
* search for Spring
* need to install Spring Tools 4 **and** Spring Tools 3 Add-On
* install / restart / repeat for each plugin
* this will install Spring Tools 4 **and** Spring Tools 3 Add-On **and** Spring Tools 3
* confirm install worked: Window > Perspective > Open Perspecitive > Other
* you should see Spring there .. change perspective if you like, then switch back to Java EE perspecitve

# Dynamic Web Project: configure SpringMVC 
* lm03-04_cps278_mvc_1-DynamicWebProject_KXy6Rya8wqM
* Dispatcher Servlet is a front controller 
* it provides single entry point into the web application
* it used MAPPING HANDLERS and to forward requests to SpringMVC controllers 

### Spring MVC configuration steps
* lm03-04_cps278_mvc_1-DynamicWebProject_KXy6Rya8wqM @ 02:30

#### make spring-mvc-demo Dynamic Web Project
* File > New > Dynamic Web Project
* Project name: spring-mvc-demo
* ensure Target runtime is Apache Tomcat 9.0, everything else left as default
* click next until "Generate web.xml" page .. make sure this is checked
    * web.xml is in webapp/WEB-INF
    * will need to edit/change this file later
* make a new package for demo: edu.wccnet.ckingdon.springMVC.controller

#### convert to Maven project
* right-click on project, Configure > Convert to Maven Project
* Group Id: edu.wccnet.ckingdon
* Artifact Id: spring-mvc-demo (should already be there)
* Packaging: WAR file (Web Archive)
* now **pom.xml** is created

#### add Maven dependencies (SpringMVC, JSTL)
* open pom.xml
* go to Dependencies tab below for GUI .. easier to copy/paste to pom.xml
* instead, go to **mvnrepository.com**
    * search: spring mvc .. pick Spring Web MVC from results .. pick recent version
    * copy the <dependency> block and put it in the project's pom.xml
    * after </build>, add <dependencies> block
    * paste the dependency from mvnrepository here
    * all other go here too
* back to mvnrepository.com, search "jstl", pick the "javax.servlet" hit (usually second on the list)
    * add this dependency in pom.xml
* check project's Maven Dependencies folder for the dependencies you just added

#### create and config SpringMVC Dispatcher Servlet
* src > main > webapp > WEB-INF > web.xml
* (could be in differnet location depending on version)
* java web apps use a deployment descriptor file to define how URLs will map to services
* ```<welcome-file-list>```
* these are the files that the server looks for in order
* add the dispatcher servlet to this file
* right-click on web.xml, New > Other > Web > Servlet (or serach "servlet")
* check "Wse an exising Servlet class or JSP" > Browse > DispatcherServlet
* qualifier tells you where it will get the servlet class
* can check for DispatcherServlet.class in Maven Dependencies > spring-webmvc > org.springframework.web.servlet

#### setup URL mappings to Dispatcher Servlet
* web.xml: ```<servlet-name>``` must be same in both ```<servlet>``` and ```<servlet-mapping>``` blocks
* change: ```<url-pattern>/</url-pattern>``` .. this way all web request will be handled by this dispatcher servlet .. i.e. there are no "sub-dirs"

#### create Spring bean configuration file
* lm03-04_cps278_mvc_1-DynamicWebProject_KXy6Rya8wqM @ 14:14
* this file holds the parameters that directs Spring to use beans, annotations/component scan
* default file name matches dispatcher but with "-Servlet" appended
* name will follow "servletname-servlet.xml"
* contains all specific Spring web MVC components
* now change ```<servlet-name>```, in both ```<servlet>``` and ```<servlet-mapping>``` blocks, to "Dispatcher" to prevent confusion/repetitive use of "servlet" in file names
    * this way, Spring looks for Dispatcher-servlet.xml rather than DispatcherServlet-servlet.xml
* WEB-INF > right-click > New > Other > Spring > Spring Bean Configuration File
* highlight WEB-INF
* File name: Dispatcher-servlet.xml
* open the file > Namespaces tab > context (for annotations/component scan)
* => "context" tab 
* right-click on beans > Insert context:component-scan
* point to base-package: edu.wccnet.ckingdon.springMVC.controller (from first step)
* Namespaces > mvc
* => "mvc" tab
* right-click on beans > Insert annotation-driven

#### configure SpringMVC ViewResolver
* shows where the page is located .. map where the page will be found
* Dispatcher-servlet.xml > "mvc" tab > New Bean
* Id: viewResolver
* Class > Browse and find InternalResourceViewResolver
* now setup controller, then return to configure the bean here

### Controllers
* lm03-05_cps278_mvc_2-RvC6lwkgmg0.mp4
* create controller class so you can use @Controller annotation in code
    * create class named Hello, open it, add @Controller above class
    * import org.springframework.stereotype.Controller; (ctrl-space to add this)
* define controller method
    * public method that returns a string value, can take any number of parameters
    * public String helloPage() {}
* add request mapping to the controller method @RequestMapping("/")
* return view name (prefix and suffix in config file)
    * return name of view file from class: return "hello", which will return hello.jsp
```
package edu.wccnet.ckingdon.springMVC.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class Hello {
	@RequestMapping("/")
    public String helloPage() {
    	return "hello";
    }
}
```
### Views
* lm03-05_cps278_mvc_2-RvC6lwkgmg0.mp4 @ 04:27
* now you need a **view** that goes with ```return "hello";```
* WEB-INF > make folder named "views"
* right-click views > New > JSP File > hello.jsp
* open hello.jsp
```
<body>
Hello World
</body>
```
* right-click project > Run As > Run On Server
* go to http://localhost:8080/spring-mvc-demo
* 404 because no prefix/suffix is not yet setup in Dispatcher-servlet.xml
* edit Dispatcher-servlet.xml > mvc tab > right-click viewResolver bean > insert property element
    * name: prefix, value: /WEB-INF/views/
    * name: suffix, value: .jsp
```
	<bean id="viewResolver"
		class="org.springframework.web.servlet.view.InternalResourceViewResolver">
	
	<property name="prefix" value="/WEB-INF/views/"></property>
	<property name="suffix" value=".jsp"></property>
	
	</bean>
```

#### Mapping: Servlet vs Request
* Servelet Mapping: Dispatcher-servlet.xml maps URLs/URL pattern to servlets
* Request Mapping: in java controller class, using @RequestMapping annonation
* can change @RequestMapping to custome: @RequestMapping("/hello"), then provide browser with updated URL


### JSTL
* lm03-06a_cps278_jstl_intro-ST34rNR5xFs.mp4
* JSTL = JSP Tag Libraries
* JSP = Java Server Pages
* lets you do looping, conditional formatting, etc
* add Core JSTL Namespace to top of .jsp file

```
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
```
* there are other tag lines to access other JSTL functionality
* review lm03 videos and docs to learn about common tags and how they function
    * form tags, dropdowns, radio buttons, form validation, regex




























