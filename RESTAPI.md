Soap 
  - security out of box
  - interface based schema definition using wsdl
  - slower than rest
  - only xml content
  - tight coupling of client since strict schema
  - supports async also

rest 
  - only sync
  - no security out of box, 
  - stateless, 
  - session management not out of box, 
  - multiple content type is supported
  - versioning of API
  - GET,PUT,POST,DELETE,PATCH,OPTIONS
  - idempotent
  - URI - URN, URL
  - SSL/TSL
  - ERROR codes
    - 1 (info), 2(sucess), 3(redirect), 4(client side error), 5(server side error)
    - description
  - Pagenation for huge data
    - use page object from spring - page number and size
  - JAX-RS - API for REST
  - HTTP Basic authentication
      - Authorization header contains username,password encoded
  - JAXRS (Jersey or RESTEasy) and JAXWS difference , Jackson jar for json parsing,annotation,binding,serialize and deserialize 
  - Annotations
    - @Path
    - @GET, @POST, @PUT, @DELETE, @HEAD
    - @PathParam,@QueryParam
    - @Produces,@Consumes
  - Spring annotations
    - @RestController
    - @GetMapping,@PostMapping,@deleteMapping
    - @Controller  and @RESTController difference
    - @PathVariable
  
  - GET - have limitations on the URL length and its not encrypted, use when we need caching (can be done by setting attributes on headers cache,refresh,age)
  - POST - body is encrypted so use for get API which requires bigger input and needs encryption because of sensitive data


  - OpenAPI 
    - bean needs to be configured with group,URL endpoints
    - open api dependency generates the json schema openAPI.json automatically from it
    - this configuration of building the openapi bean is placed in reuseable framework libraries and re-used in all microservices
