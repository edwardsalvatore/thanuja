<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-api</artifactId>
    <version>0.11.2</version>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-impl</artifactId>
    <version>0.11.2</version>
</dependency>
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;
import java.util.Date;
public String generateJwtToken() {
    String subject = "your-subject"; // Replace with your subject
    long expirationMillis = System.currentTimeMillis() + 3600000; // 1 hour
    Date expirationDate = new Date(expirationMillis);

    String secretKey = "your-secret-key"; // Replace with your secret key

    String token = Jwts.builder()
            .setSubject(subject)
            .setExpiration(expirationDate)
            .signWith(SignatureAlgorithm.HS256, secretKey)
            .compact();

    return token;
}
String jwtToken = generateJwtToken();
System.out.println("Generated JWT Token: " + jwtToken);
