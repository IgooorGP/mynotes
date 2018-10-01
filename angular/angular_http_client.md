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
## Observable Types

HttpClient or the older http module allows us to make HTTP requests. However, such requests are, in fact, performed asynchronously in a way that they do not return the plain response or body object right away! Instead, they return an **Observable<<TT>>**.

* In order to use the response, a method in another service or component must **subscribe** to the observable object;
* This subscribe() method accepts a callback which will be executed once the HTTP response has been received!

### Async GET example

1. First we create a method that performs the async HTTP request which returns an observable object.

* ANY components that rely on this service will be able perform the HTTP request by invoking the getData method and subscribing to it.

```typescript
{

export class Service {

    // http client injection
    constructor (private httpClient: HttpClient) {}

    getData() {
        // returns an Observable<> HttpResponse
        return this.httpClient.get("...", {
            response: 'response',  
            responseType: 'json'
        });
    }        
}
```

* With the older http module, one could invoke as follows in order to return a json object once we subscribe!

```typescript
    this.http.get().map(response => return response.json();)
```



2. In a component which relies on this service, we create a method that will be BOUND to an event such as a click:

* After this component's method is FIRED due to the event, the service's getData() is performed and we subscribe to use its response!

* Inside the subscription, we can set the component class variable which may propagate this data back to the template!

```typescript
{
@Component({
    ...
    providers: [Service] // provides the service
})
export class Component {

    // service injection into the component
    constructor (private service: Service) {}

    // method to be BOUND to an event (click, etc.)
    onGet() {
        this.service.getData() // invokes the async http from service!
            .subscribe(
                (list) => {
                    // bind the returned data to the component!!
                    this.list = list;
                },
                (error) => console.log(error)
            );
    }      
}
```

