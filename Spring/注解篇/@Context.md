### @Context注解

源码分析,改注解来源于JAX-RS API，不属于Spring注解

```
/**
 * This annotation is used to inject information into a class
 * field, bean property or method parameter.
 *
 * @author Paul Sandoz
 * @author Marc Hadley
 * @see Application
 * @see UriInfo
 * @see Request
 * @see HttpHeaders
 * @see SecurityContext
 * @see javax.ws.rs.ext.Providers
 * @since 1.0
 */
@Target({ElementType.PARAMETER, ElementType.METHOD, ElementType.FIELD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Context {
}
```

应用场景如下几种情况

```
@Context
  private HttpServletRequest request;
```

```
 public Response getListing(@Context HttpHeaders headers, @Context UriInfo uriInfo,
      @PathParam("type") String type) throws JsonProcessingException
```

```
@Context
  protected UriInfo uriInfo;
```

```
@POST
@Path(value = "logout")
public void logout(@Context HttpServletRequest request) {
logout();
}
```