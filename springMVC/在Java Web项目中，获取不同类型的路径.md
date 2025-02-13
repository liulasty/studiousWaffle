在Java Web项目中，获取不同类型的路径是常见的需求，比如获取项目根路径、上下文路径（Context Path）、Servlet路径、Web资源路径等。这些路径的获取方式依赖于你所使用的技术栈（如Servlet API、Spring MVC等）以及你的具体需求。以下是一些常见路径的获取方式：

### 1. 上下文路径（Context Path）

上下文路径是部署在服务器上的Web应用的根路径。在Servlet中，可以通过`HttpServletRequest`对象的`getContextPath()`方法来获取。

```java
java复制代码String contextPath = request.getContextPath();
```

### 2. Servlet路径（Servlet Path）

Servlet路径是请求URL中对应于Servlet的部分，即URL中去掉上下文路径和查询字符串后剩余的部分。在Servlet中，可以通过`HttpServletRequest`对象的`getServletPath()`方法来获取。

```java
java复制代码String servletPath = request.getServletPath();
```

### 3. 路径信息（Path Info）

路径信息是在Servlet路径之后但在查询字符串之前的额外路径信息。这通常用于映射到Servlet的特定资源或方法。在Servlet中，可以通过`HttpServletRequest`对象的`getPathInfo()`方法来获取。

```java
java复制代码String pathInfo = request.getPathInfo();
```

### 4. 请求URI（Request URI）

请求URI是请求的完整URI（统一资源标识符），包括上下文路径、Servlet路径、路径信息和查询字符串（但不包括协议和主机名）。在Servlet中，可以通过`HttpServletRequest`对象的`getRequestURI()`方法来获取。

```java
java复制代码String requestURI = request.getRequestURI();
```

### 5. 请求URL（Request URL）

请求URL是请求的完整URL，包括协议、主机名、端口号、上下文路径、Servlet路径、路径信息和查询字符串。在Servlet中，可以通过`HttpServletRequest`对象的`getRequestURL()`方法获取一个`StringBuffer`对象，然后转换为字符串。

```java
java复制代码StringBuffer requestURL = request.getRequestURL();  String url = requestURL.toString();
```

### 6. 项目根路径（对于文件访问）

如果你需要访问项目内的文件（如配置文件、图片等），通常需要知道项目的根路径或类路径（Classpath）。在Servlet中，项目根路径（即Web应用的根目录）不是直接暴露给Servlet的，因为Servlet运行在服务器上，而文件访问通常基于服务器文件系统。不过，你可以使用`ServletContext`的`getRealPath(String path)`方法来获取Web应用内相对于Web内容目录的路径的实际文件系统路径。

```java
java复制代码String realPath = getServletContext().getRealPath("/WEB-INF/config.properties");
```

注意，`getRealPath()`方法在Servlet 3.0及更高版本中可能返回`null`，特别是在嵌入式Servlet容器（如Tomcat的嵌入式版本）中，因为某些容器可能不将Web内容目录映射到文件系统的实际目录。

对于类路径上的资源（如JAR文件中的文件），应该使用`Class.getResource()`或`ClassLoader.getResource()`方法来访问。

```java
java复制代码InputStream inputStream = getClass().getResourceAsStream("/config.properties");
```

这些是在Java Web项目中获取不同类型路径的基本方法。具体使用哪种方法取决于你的具体需求和所使用的技术栈。