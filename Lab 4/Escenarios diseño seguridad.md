# Scenarios for Security Design

## Scenario 1: Pseudo-Code for Authentication System

```
FUNCTION authenticateUser(username, password):
  QUERY database WITH username AND password
  IF found RETURN True
  ELSE RETURN False
```

### Reporte

Analizando el pseudocodigo, se observa que la implementación permite SQL Injection además de que no se ve que se manejen contador de intentos o por consiguiente no se maneja algún log de esos intentos fallidos como para monitorear o detectar algo inusual. Del mismo modo al no descifrar la password y compararla si la encontró o no directamente, se deduce que esta esta en texto plano por lo que también se considera una vulnerabilidad. Para atacarlas se tendría que:

1.	Utilizar PreparentStatement (en java) para prevenir SQL Injection
2.	Colocar algún bloqueo por intentos fallidos y logs en el método para monitorear
3.	Implementar algún proceso hashing para la contraseña





## Scenario 2: JWT Authentication Schema
```
DEFINE FUNCTION generateJWT(userCredentials):
  IF validateCredentials(userCredentials):
    SET tokenExpiration = currentTime + 3600 // Token expires in one hour
    RETURN encrypt(userCredentials + tokenExpiration, secretKey)
  ELSE:
    RETURN error
```

### Analisis
se observa que se pueden realizar algunas recomendaciones como agregar la firma del JWT lo que permite modicar el token sin deteccion y la validacion de tiempo de expiracion del token

### Resultado

```
public static String generateJWT(UserCredentials userCredentials) {
        try {
            if (validateCredentials(userCredentials)) {
                long tokenExpiration = currentTime() + 3600; // Token expires in one hour
                Algorithm algorithm = Algorithm.HMAC256(SECRET_KEY);
                String token = JWT.create()
                        .withClaim("userID", userCredentials.getUserID())
                        .withExpiresAt(new Date(tokenExpiration * 1000))
                        .sign(algorithm);
                return token;
            } else {
                return "Invalid credentials";
            }
        } catch (Exception e) {
            LOGGER.log(Level.SEVERE, "Error generating JWT", e);
            return "Error generating token";
        }
    }

    public static String verifyJWT(String token) {
        try {
            Algorithm algorithm = Algorithm.HMAC256(SECRET_KEY);
            DecodedJWT decoded = JWT.require(algorithm)
                    .build()
                    .verify(token);
            return decoded.getPayload();
        } catch (TokenExpiredException e) {
            return "Token expired";
        } catch (SignatureVerificationException | JWTDecodeException e) {
            return "Invalid token";
        }
    }
```
## Scenario 3: Secure Data Communication Plan

Outline for Data Protection:

```
PLAN secureDataCommunication:
  IMPLEMENT SSL/TLS for all data in transit
  USE encrypted storage solutions for data at rest
  ENSURE all data exchanges comply with HTTPS protocols
```

### Resultado
```
PLAN secureDataCommunication:
  IMPLEMENT SSL/TLS for all data in transit
  CONFIG web server
  USE encrypted storage solutions for data at rest
  USE strong encryption algorithms
  USE system for managing encryption and decryption
  ENSURE all data exchanges comply with HTTPS protocols
  UPDATE applications all to https
  USE network whitelist
```
