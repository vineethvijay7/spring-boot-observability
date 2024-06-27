# spring boot example

Source: <https://spring.io/guides/gs/serving-web-content>

## Build It

```bash
# Pre-req: you need to have `sdk`: https://sdkman.io/install. 
# Again this is upto you. If you have the correct java runtime already, feel free to move to the build part.
sdk use java 17.0.9-tem # to select correct java runtime

## Build the jar using gradle. You can use maven if you want, then the path to the jar needs to be updated in the Dockerfile
./gradlew bootJar
```

Note: you can use maven if you want. Then the path to the jar needs to be updated in the Dockerfile

## Docker run the app alone

```bash
docker run --rm -it -p 8080:8080 $(docker build -q .)
```

## Check the endpoints

<http://localhost:8080>
<http://localhost:8080/greeting>
<http://localhost:8080/greeting?name=User>
