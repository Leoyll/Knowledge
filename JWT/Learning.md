# /JWT/Learning
## Example

**<font size="4" color="red">TODO Server1生成的JWT token，如何在Server2有效？</font>**

```java=
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;
import io.jsonwebtoken.security.Keys;
import java.security.Key;

@Service
public class TokenService {
    public String generate(Payload payload) { //Payload: custom PO Class
        String secretKey = "jwtKeyExample";
        Key key = Keys.hmacShaKeyFor(secretKey.getBytes());

        Map<String, Object> claims = new HashMap();
        claims.put("identity", payload);

        return Jwts.builder().setHeaderParam("typ", "JWT")
                .setClaims(claims)
                .setExpiration(Date.from(new Date().toInstant().plus(Duration.ofHours(1))))
                .signWith(key, SignatureAlgorithm.HS256)
                .compact();
    }
}

/**    ####################################################        */

@Service
public class AuthWebService {
    private final TokenService tokenService;

    @Autowired
    public AuthWebService(TokenService tokenService) {
        this.tokenService = tokenService;
    }
    
    public String getToken() {
        Payload payload = new Payload();
        // Initialize payload object
        return tokenService.generate(payload);
    }
}

/**    ####################################################        */

@Controller
@RequestMapping("/jwtExample")
public class JwtExampleController extends BaseController {
    @Autowired
    AuthWebService authWebService;

    @ResponseBody
    @GetMapping(value = "/token")
    public String getToken(){
        return authWebService.getToken();
    }
}
```