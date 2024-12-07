# Java Code Analysis!?!
Description:BookShelf Pico, my premium online book-reading service.
I believe that my website is super secure. I challenge you to prove me wrong by reading the 'Flag' book!

### Solution:
In order to take the flag, we need to access the book call flag
click it and use burp suite intercept it to get the details of the http request
we have:
```
GET / HTTP/1.1
Host: saturn.picoctf.net:59947
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0
Accept: application/json, text/plain, */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJyb2xlIjoiRnJlZSIsImlzcyI6ImJvb2tzaGVsZiIsImV4cCI6MTczNDE3NzgwNSwiaWF0IjoxNzMzNTczMDA1LCJ1c2VySWQiOjEsImVtYWlsIjoidXNlciJ9.GyRgZUnVOIyRDlpG745Z-2kRh75GZFJFO9BiN48KNjM
Connection: keep-alive
Referer: http://saturn.picoctf.net:59947/
```
we can see it use Bearer token for the authenication

according to the hints, we need to find the secret key of JWT
so, take a look to the JWTservice.java 
```
   @Autowired
    public JwtService(SecretGenerator secretGenerator){
        this.SECRET_KEY = secretGenerator.getServerSecret();
    }
```
the secret_key is come from secretGenerator.java, so go to secretGenerator.java
```
@Service
class SecretGenerator {
    private Logger logger = LoggerFactory.getLogger(SecretGenerator.class);
    private static final String SERVER_SECRET_FILENAME = "server_secret.txt";

    @Autowired
    private UserDataPaths userDataPaths;

    private String generateRandomString(int len) {
        // not so random
        return "1234";
    }

    String getServerSecret() {
        try {
            String secret = new String(FileOperation.readFile(userDataPaths.getCurrentJarPath(), SERVER_SECRET_FILENAME), Charset.defaultCharset());
            logger.info("Server secret successfully read from the filesystem. Using the same for this runtime.");
            return secret;
        }catch (IOException e){
            logger.info(SERVER_SECRET_FILENAME+" file doesn't exists or something went wrong in reading that file. Generating a new secret for the server.");
            String newSecret = generateRandomString(32);
            try {
                FileOperation.writeFile(userDataPaths.getCurrentJarPath(), SERVER_SECRET_FILENAME, newSecret.getBytes());
            } catch (IOException ex) {
                ex.printStackTrace();
            }
            logger.info("Newly generated secret is now written to the filesystem for persistence.");
            return newSecret;
        }
    }
}
```
we can assume that the secret key is 1234 because we can't access the file
next, we can look at the Role.java beacause we need to change our role
```
public class Role {
    @Id
    @Column
    private String name;

    @Column
    private Integer value; //higher the value, more the privilege. By this logic, admin is supposed to
    // have the highest value
}
```
get a hint that is priveilege is related to the higher value
now we can go to JWT.io to decode the token we got before
![image](https://github.com/user-attachments/assets/ebd5bf33-f3f4-4b46-90c8-cf08568af4c8)
change the email and role to admin and userID to 2
also, enter the secret key 1234
copy the token and playload
open inspector, change the local storage parameters with token and playload
then refresh, get the flag:))
