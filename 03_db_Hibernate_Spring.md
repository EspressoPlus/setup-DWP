# required dependencies

* spring-webmvc, which facilitates: 
    * Model: communicates with database via business logic + DAO, returns model as result
    * View: jsp files + jstl
    * Controller: Dispatcher Servlet, Handler mapping, annotations/URL mapping
* jstl: java server pages tag library support
* hibernate-core: using Hibernate to connect to db
* mysql-connector-java: using MySQL
* c3pO: implements JDBC connection pools (min/max size, idle times, etc)
* spring-orm: handles how Spring handles ORM transactions (not directly related to Hibernate)
