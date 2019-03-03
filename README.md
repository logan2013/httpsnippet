# HTTP Snippet

> HTTP Snippet port for java. See [the original node port](https://github.com/Kong/httpsnippet). Supports *many* languages & tools including: `cURL`, `HTTPie`, `Javascript`, `Node`, `C`, `Java`, `PHP`, `Objective-C`, `Swift`, `Python`, `Ruby`, `C#`, `Go`, `OCaml` and more!
> The motivation behind porting this is using it for generating snippets in swagger and redocs  

The project is still in development phase. 

- [ ] Documentation
- [ ] Tests
- [ ] Releasing to maven

##  Usage
Enable maven snapshots in `~/.m2/settings.xml`
```xml
<profiles>
  <profile>
     <id>allow-snapshots</id>
        <activation><activeByDefault>true</activeByDefault></activation>
     <repositories>
       <repository>
         <id>snapshots-repo</id>
         <url>https://oss.sonatype.org/content/repositories/snapshots</url>
         <releases><enabled>false</enabled></releases>
         <snapshots><enabled>true</enabled></snapshots>
       </repository>
     </repositories>
   </profile>
</profiles>
```
 Then add this `dependency` to  `pom.xml`

```xml
<dependency>
     <groupId>io.github.atkawa7</groupId>
     <artifactId>httpsnippet</artifactId>
     <version>0.0.1-SNAPSHOT</version>
</dependency>
```

```java
public class Main {
    public static void main(String[] args) throws Exception {
        List<HarHeader> headers = new ArrayList<>();
        List<HarQueryString> queryStrings = new ArrayList<>();

        User user = new User();
        Faker faker = new Faker();
        user.setFirstName(faker.name().firstName());
        user.setLastName(faker.name().lastName());


        HarPostData harPostData =
                new HarPostDataBuilder()
                        .withMimeType(HttpHeaders.APPLICATION_JSON)
                        .withText(ObjectUtils.writeValueAsString(user)).build();

        HarRequest harRequest =
                new HarRequestBuilder()
                        .withMethod(HttpMethod.GET.toString())
                        .withUrl("http://localhost:5000/users")
                        .withHeaders(headers)
                        .withQueryString(queryStrings)
                        .withHttpVersion(HttpVersion.HTTP_1_1.toString())
                        .withPostData(harPostData)
                        .build();

        HttpSnippet httpSnippet = new HttpSnippetCodeGenerator().snippet(harRequest, Language.JAVA);

    }

    @Data
    static class User {
        private String firstName;
        private String lastName;
    }
}
```


## License

[Apache 2.0](LICENSE) &copy; [atkawa7](https://github.com/atkawa7/httpsnippet)

[license-url]: https://github.com/atkawa7/httpsnippet/blob/master/LICENSE
