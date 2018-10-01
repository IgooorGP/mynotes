# The New HTTP Client

Angular has released a new HTTP CLIENT which is more modern than the previous one! The new client has new features such as interceptors! The rest is pretty much same.

## Importing the client at the AppModule

Just like other module, we must tell angular that we want to use the new HttpClient by providing a new import at the imports array.

```typescript 

import { HttpClientModule } from '@angular/common/http';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    HttpClientModule  // the new HttpClient module
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

## Declaring the service dependency on another service

Since Angular knows that we are using the HttpClientModule, we can know INJECT it in other services!

```typescript 
@Injectable()
export class OneService {

    // declares the dependency of the HttpClient service
    constructor(private httpClient: HttpClient) {}

    someMethod() {
        // uses the INJECTED client service
        this.httpClient.get("...");
    }
}
```

When using services, for components we declare the provider inside the @Component decorator.

When it comes down to services that rely on other services, then we use the @Injectable() decorator.

## Datatype of the response

The old client had a general object Response that we could manipulate to extract data from the HTTP response that we received.

However, the new client allows us to specify the response types!

### Old http client:

* Returns a general Response object

```typescript 
    someMethod() {
        this.http.get("...")
            .map(
                (response: Response) => {
                    // get the response as a JSON
                    response.json()
                }
            )
    }
```

### New http client: 

* Accesses body HTTP data right away! Of course we can customize the response to get access to the HTTP headers such as HTTP status code, etc.

* Default is to access body data;
* Default Content-Type is application/json.

```typescript 
    someMethod() {
        this.httpClient.get("...")
            .map(
                // the default extracts access to BODY data automatically
                (myList) => {
                    // the default is already a JSON Content-type!
                }
            )
    }
```

Typed Response Data:

* We can specify the response **type** if we are accessing the BODY directly!

```typescript 
    someMethod() {
        // type declaration allows typescript to infer myList type
        this.httpClient.get<Recipe[]>("...")
            .map(
                (myList) => {
                    // myList is of type Recipe[]
                }
            )
    }
```

Specifying options to the HTTP request:

```typescript 
    // object as options
    this.httpClient.get<Recipe[]>("...", {})

    // body, headers manipulation
    this.httpClient.get<Recipe[]>("...", {
        body: ...
        headers: ...
    })

    // getting full object response --> HttpResponse object
    this.httpClient.get<Recipe[]>("...", {
        response: 'response',  // full object, default == body
        responseType: 'json'  // default, but could be text, etc.
    })
```

**Full** Response object (HttpResponse):

```typescript
{
    body: ...  // depends upon the the responseType requested!
    headers: {  // HttpHeaders
        status: 200,
        ok: true,
        ur: ""
    }

} 
```