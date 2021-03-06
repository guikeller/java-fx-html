Java FX HTML - Bridge
============

Rather than developing apps using FXML use HTML, CSS and JS.

Make the most of Web technologies and develop a responsive modern looking desktop app.<br> 
Utilize HTML / CSS / JS thanks to the JavaFX WebView / WebEngine (Nashorn) and JSObject.

Java 1.8 - no extra deps

```java
WebEngine webEngine = this.webView.getEngine();
this.fxHtml = new CoreJavaFxHtml(webEngine);
this.fxHtml.registerFxHtml("javaFxHtml");
``` 

Ready to integrate with Spring (optional)

```java
WebEngine webEngine = this.webView.getEngine();
this.applicationContext = SpringContextHolder.getApplicationContext();
this.fxHtml = new SpringJavaFxHtml(webEngine, this.applicationContext);
this.fxHtml.registerFxHtml("javaFxHtml");
```

Ready to integrate with Guice (optional)
```java
WebEngine webEngine = this.webView.getEngine();
this.injector = GuiceInjectorHolder.getInjector();
this.fxHtml = new GuiceJavaFxHtml(webEngine, this.injector);
this.fxHtml.registerFxHtml("javaFxHtml");
```

On your HTML
```html
<html>
    <body>
        <form id="notNecessary" action="#">
            <input type="text" id="name" value="">
            <input type="number" id="age" value="">
            <button onclick="submitValues()">Save</button>
        </form>
    </body>
    <script type="application/javascript">
        function submitValues() {
            /* MyController#saveUserInfo(UserInfoModel) is invoked with the 'Model' automagically populated */
            var result = javaFxHtml.invokeController("com.app.controller.MyController", "saveUserInfo");
            console.log("Java Returned: " + result);
        }
    </script>
</html>
```

Controller:
```java
public class MyController{
   @FxControllerMethod
   public String saveUserInfo(UserModel user) {
       System.out.println("NAME: " + model.getName());
       System.out.println("AGE: " + model.getAge());
       // Think of the possibilities with JSON / Jackson
       return "{\"result\": \"success\", \"timestamp\": \"" + Instant.now() + "\"}";
   }
}
```

Model:
```java
@Data // Project Lombok
public class UserModel {
    // You can also create your own converter by implementing the Converter interface
    @FxHtmlField(elementId = "age", converter = IntegerConverter.class)
    private Integer age;

    @FxHtmlField(elementId = "name")
    private String name;

}
```

#### Main Classes (com.github.guikeller.javafxhtml) [entry points]:
 * CoreJavaFxHtml
 * SpringJavaFxHtml
 * GuiceJavaFxHtml
 
#### Annotations (com.github.guikeller.javafxhtml.annotations):
 * FxHtmlInitMethod (utilized to pre-populate the first html screen)
 * FxControllerMethod (utilized to mark the 'method' that can be invoked in a Controller)
 * FxHtmlField (binds the POJO field to an HTML Input Element)
 
#### Converters (com.github.guikeller.javafxhtml.converters):
 * BooleanConverter
 * DateConverter (Instant)
 * DoubleConverter
 * IntegerConverter
 * StringConverter (default) 
 
#### Using as a Maven dependency
Download the latest jar from the latest release tag and install into your local repo by using the following command:
```shell
mvn install:install-file -Dfile=java-fx-html-0.0.2.jar -DgroupId=com.github.guikeller -DartifactId=java-fx-html -Dversion=0.0.2 -Dpackaging=jar
```
Alternatively install on your own managed repo - eg: Nexus, Artifactory, etc.. (company wide)<br>
Use it as dependency on your POM file - follows example below:
```xml
<dependency>
    <groupId>com.github.guikeller</groupId>
    <artifactId>java-fx-html</artifactId>
    <version>0.0.2</version>
</dependency>
```

#### Running a Sample app:

Checkout the project
```
$git clone https://github.com/guikeller/java-fx-html.git
```
Import the project into your favourite IDE and run one of the following:
```java
my.sample.app.core.main.CoreAppMain
my.sample.app.spring.main.SpringAppMain
my.sample.app.guice.main.GuiceAppMain
```
Code samples are under: src/test/java

#### Screenshots
![JavaFX HTML CSS JS PRETTY](https://raw.githubusercontent.com/guikeller/blob/master/java-fx-html-sample2.png)

![JavaFX HTML CSS JS FIREBUG](https://raw.githubusercontent.com/guikeller/blob/master/java-fx-html-sample.png)
Just think of the potential and possibilities..

#### Further info
* Disclaimer - Limitations:
1. All @FxController methods must have only one param: the "Model"
2. It does not support composition / aggregation for Model classes

* JavaFX + HTML + CSS + JS = View
* Java 8 and/or above = Controller
* Java 8 and/or above = Model

#### PRs and License
Pull Requests are more than welcome!<br>

License MIT


## Project supported by JetBrains
<p>
 Many thanks to Jetbrains for kindly providing a license for me to work on this and other open-source projects.
 <br>
 <a href="https://www.jetbrains.com/?from=7505-idea-jetty-runner">
   <img alt="Jetbrains" src="https://raw.githubusercontent.com/guikeller/blob/master/jetbrains.png" width="250" height="250">
 </a>
 <br>
 <a href="https://www.jetbrains.com/?from=7505-idea-jetty-runner">
   https://www.jetbrains.com
 </a>
 <br>
</p>

