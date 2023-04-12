# PREJECT 1.0

Detalles:

## ARCHITECTURE MVC
```
├── controllers
│   ├── AuthController.java
│   └── UserController.java
├── dtos
│   ├── AuthRequestDto.java
│   └── UserDto.java
├── entities
│   └── User.java
├── repositories
│   └── UserRepository.java
├── security
│   ├── config
│   |   └── SecurityConfig.java
│   ├── jwt
│   |   ├── JetAuthEntryPoint.java
│   |   ├── JwtRequestFilter.java
│   |   └── JwtTokenUtil.java
│   ├── payload
│   |   ├── JwtResponse.java
│   |   ├── LoginRequest.java
│   |   ├── MessageResponse.java
│   |   └── RegisterRequest.java
│   └── service
│       └── UserDetailsServiceImpl.java
├── services
│   ├── UserService.java
│   └── JwtServiceTest.java
├── config
│   └── WebConfig.java
── test
    └── controllers
       ├── AuthControllerTest.java
       └── UserControllerTest.java


```
## JWT y SESSION

https://jwt.io

Es un estandar abierto que permite transmitir informacion entre dos partes:

JSON web Token
### Funcionamiento Session
1. Cliente envia una peticion a un servidor (/api/login)
2. Servidor valida el username y la password, Si no son validos devolvera una respuesta 401 unauthorized
3. Servidor valida el username y la password, Si son validos entonces se almacena el usuario en session
4. Se genera una cookie en el Cliente
5. En proximas peticiones se comprueba que el cliente esta en session

Desventajas:

* La informacion de la session se almacena en el servidor, lo cual consume recursos.

### Funcionamiento JWT
1. Cliente envia una peticion a un servidor (/api/login)
2. Servidor valida el username y la password, Si no son validos devolvera una respuesta 401 unauthorized
3. Servidor valida el username y la password, Si son validos entonces genera un Token JWT utilizando una secret key
4. Servidor devuelve el token JWT generado
5. Cliente envia peticiones a los endpoint REST del servidor utilizando el token JWT en las cabeceras
6. Servidor valida el token JWT en cada peticion y si es correcto, permite el acceso a los datos.


Ventajas:

* El token JWT se almacena en el Cliente, consumiendo menos recursos en el servidor, permitiendo mejor escalabilidad

Desventajas:

* El token esta en el navegador, sin poder invalidarlo antes de la fecha de expiracion asiganada cuando se creo
    * Lo que se realiza es dar la opcion de logout, lo cual simplemente borra el token.


### Estructura del token JWT


3 Partes separadas por un punto (.) y codificadas en base 64 cada una:

1. Header

```json
{
  "alg": "H5512",
  "typ": "JWT"
}
```
2. Payload (informacion, datos del usuario, no sensibles)

```json
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true
}
```

3. Signatura

```
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)
```


Ejemplo del token generado:

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

El User-Agent envía el token JWT en las cabeceras:

```
Authorization: Bearer <token>
```




## TEST
 Utilizo Mockito para Testear Service y controller a pesar de que es posible hacerlo con Spring Test me llevo mejor haciendolo con Mockito
### Estructura MOCKITO
1.  MVC builder
```java
    private MockMvc mockMvc;
```
2. Headers
```java
    private final HttpHeaders headers = new HttpHeaders();

```
3. Objects
```java
    private static final User USER_1 = new User("user","password");
    private static final User USER_2 = new User("user1","password1");
    private final List<User> records= new  ArrayList<User>(Arrays.asList(USER_1, USER_2));
  
```
1. Mock e Inject
```java
    @Mock
    private LaptopService laptopService;

    @InjectMocks
    private LaptopController laptopController;

```
2. Build
```java
    @BeforeEach
    public void setUp(){
        MockitoAnnotations.initMocks(this);
        this.mockMvc = MockMvcBuilders.standaloneSetup(laptopController).build();
        headers.add("Authorization", "laptop-value-45xx23");
    }
```
### Services y Controllers
* register
  * register
  * registerNull
* login
  * login
  * loginNull
* save (Entity2)
  * save
  * saveNull
  * saveUnauthorized
* findAll (Entity2)
  * findAll
  * findAllNull
  * findAllUnauthorized
* findOne (Entity2)
  * finOne
  * findOneNullId
  * findOneUnauthorized
* update (Entity2)
  * update
  * updateNull
  * updateUnauthorized
* delete (Entity2)
  * delete
  * delteNullId
  * deleteUnauthorized

_No se incluiran en el proyecto: Recuperacion de contraseña ni de usuario_


## CONFIG SPRING

Crear proyecto Spring Boot con:

* Spring 2.5.5
* Spring Security
* Spring Web
* Spring boot devtools
* Spring Data JPA
* H2
* Swagger (manual)
```xml
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-boot-starter</artifactId>
    <version>3.0.0</version>
</dependency>
```
* Mockito (manual)
```xml
<dependency>
    <groupId>org.mockito</groupId>
    <artifactId>mockito-all</artifactId>
    <version>2.0.2-beta</version>
    <scope>test</scope>
</dependency>
```

* Dependencia jwt (manual)
```xml
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt</artifactId>
    <version>0.9.1</version>
</dependency>
```

* PROPERTIES (H2)
```
spring.jpa.show-sql=true
spring.datasource.url=jdbc:h2:file:C:/data/sample
spring.datasource.username=sa
spring.datasource.password=
spring.datasource.driverClassName=org.h2.Driver
#spring.jpa.hibernate.ddl-auto=creat
spring.jpa.hibernate.ddl-auto=update
spring.sql.init.mode=always
spring.jpa.defer-datasource-initialization=true
```
## ERRORES
1. Al inciar lanza un error si: Agrego Properties para H2 e incluyo Swagger

## LICENCIA 
```
No se incluye en este proyecto -2023
```
