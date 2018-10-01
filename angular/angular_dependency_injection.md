# Angular Services

## Services

* Instantiating services directly in components is NOT the right way when using angular;
* Services should be instantiated by the providers instead;
* Providers array is a property from the @Component annotation;
* Services are declared at the classes constructors!

```javascript
@Component({
    ...
    providers: [ServiceTypeClass]  // Component Hierarchy
})
export class Component {
      
    // this declares a service dependency --> angular
    // will use the provider to INJECT it on this component
    constructor (private service: ServiceTypeClass) {}
    
    ngOnInt() {
        // must initialization code goes here --> leave constructor for DI
    }
}
```

* As noted above, the TYPE declaration of the service is essential **both** at the  providers and at the constructor signature;
* Services dependencies should be declared at the class's constructor;
* Given a provider and the dependency on the constructor --> Angular will INJECT the dependency of the component.

## Hierarchical Injection

* Angular uses a **hierarchical** way to instantiate services for DI;
* Depending on WHERE the service is declared, children components may received the SAME instante (~singleton);
* Service instances go down its hierachical TREE! AppModule --> AppComponent --> Component ...

### Places to declare dependencies

1. AppModule --> SAME service instance for WHOLE application;
2. AppComponent --> SAME service instance for all components (top-level component but NOT other services);
3. OtherComponentes --> SAME service instance for this component and its children components

By providing the same service AGAIN --> another instance is given due to overriding!

## Injecting Services INTO services

Some services may rely on another services! To inject those, we declare the dependencies at the constructor in the same way as we do for components!

* However, --> services or other classes that have dependencies to BE injected require a new annotation: @Injectable()!

* This provides metadata for angular to know that there's something that will be injected!

* Only add Injectable if something needs to injected. Otherwise, forget it!

```javascript
@Injectable()  // declares that this class HAS a dependency!
export class OneService {
     
    // here we declare the service dependency
    constructor (private anotherService: AntherService) {}
    
}
```

## Importante of Services

Services can make the code way cleaner and easier to maintain! It reduces the need for:

* Custom event emission;
* Event capture ($event);
* Pass data via property binding to other component we wanted the data.

### Data transmission between components (Cross-communication)

1. First, we create a component that has an event emitter!

```javascript
export class OneService {
     
    constructor () { }
    
    // declares an event emitter of a string
    emitMyEvent = new EventEmitter<string>();
}
```

2. Secondly, we create a component that relies on this SERVICE and we use the same SERVICE to emit a signal! This event will be later captured by the OTHER component to transmit data!

```javascript
@Component({
    ...
})
export class Component1 {
     
    constructor (private oneService: OneService) {}
       
    // this method emits an event FROM THE SERVICE
    emitTheEvent() {
        this.oneService.emitMyEvent("HELLO");
    }
      
}
```

3. Have ANOTHER component to SUBSCRIBE to the event emitter (observable pattern) of the SAME service! Now, this component will RECEIVE the data from the other component!

```javascript
export class Component2 {
     
    constructor () { }s 
    
    // this component must have the SAME service dependeny
    constructor(private oneService: OneService) {
        
        // here we SUBSCRIBE to the observable event from the SAME service
        // and get data from COMPONENT1 !
        this.oneService.emitMyEvent.subscribe( (status: string) => {
            console.log('received from component1 : ' + status);
        })   
    }
}
```
    