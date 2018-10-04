# Routing

Angular routes allow us to change the URL of application even though we still remain in the same index.html file as a SPA. 

## Setting the routes

Setting up routes in the AppModule is good practice since it's a starting point for the WHOLE application!

The routes have a specific **form** the angular to use them:

* path --> the endpoint (without the slash);
* component --> which component should be loaded! (CLASS TYPE).

```typescript
import { Routes, RouterModule } from "@angular/router";

// routes Routes type
const appRoutes: Routes = [
    { path: '', component: HomeComponent},  // HomeComponent (empty path ~root)
    { path: 'users', component: UserComponent},
    { path: 'servers', component: ServersComponent}
];

// AppModule
@NgModule({
    ...
    imports: [
        RouterModule.forRoot(appRoutes)  // uses router module with the ROUTES!
    ]
    ...
})
export class AppModule {}
```
## Adding the components based on the route

When using Angular's router, we dont put the components selectors right way into the html templates! We must use a **special tag**: ```<router-outlet></router-outlet>```


Using the router-outlet tag, should now apply the components defined in the routes!


## Watch for anchor tags

First of all, do NOT put the routes that we defined (appRoutes) as the href of anchor tags --> this RELOADS the application which, obviously, is not the purpose of an Angular app!

However, Angular offers two ways that DO NOT RELOAD the page:

* routerLink attribute: ```<a routerLink="/route">...</a>```
* property binding: ```<a [routerLink]="['/path', 'to', 'something']">...</a>```

The property binding is for more complex and nested routes!

And exception is raised if a route pattern is NOT FOUND!


## Navigation paths

When using routerLink attribute, one could use absolute or relative paths:

```absolute path```: /path/to/something <br>
```relative path```: that path is APPENDED to END of the current path!

For RELATIVE PATHS --> ```../lower```: allows us to go back one path level back!

## Styling active router links

When using the routerLink attribute, one can style the active link by applying dynamrically the css class to the anchor tag directive:

```html
<a routerLink="/" routerLinkActive="activeCSSClass"></a>
```
```html
<a routerLink="/route" routerLinkActive="activeCSSClass"></a>
```
```html
<a routerLink="/route2" routerLinkActive="activeCSSClass"></a>
```

However, the empty route is ALWAYS active since its always on the path! So replace:

```html
<a routerLink="/" routerLinkActive="activeCSSClass"></a>
```

with:

```html
<a routerLink="/" routerLinkActive="activeCSSClass" [routerLinkActiveOptions]="{exact: true}"></a>
```

Now, the CSS is only applied if the EXACT path is found, and not just a part of it!

## Components and Routes (Programmatically Routing -- Routing with components)

Using routerLink directive is a good a simple way to change components. However, Components themselves are also able to change routes and it may be important to do so after some complex calculation or something only then to redirect to another component!


### Inject the Router class

```typescript
import { Router } from "@angular/router";

// AppModule
@Component({
    ...
})
export class Component implements OnInit {
        
    constructor(private router: Router) {}

    onMethod() {
        // performs some COMPLEX code
            
        // redirects to the other component (array of path segments)
        this.router.navigate(['/path']);
    }
}
```

### Relative programatically routing

router.navigate does NOT know the current route --> so no relative appending unless we configure it!

```typescript
import { ActivatedRoute } from "@angular/router";

@Component({
    ...
})
export class Component implements OnInit {
    
    // inject the activated route!
    constructor(private router: Router, private activatedRoute: ActivatedRoute) {}

    onMethod() {
        // redirects to the path RELATIVE to the actual route
        this.router.navigate(['relativePath'], {relativeTo: this.activatedRoute});
    }
}
```

## Passing parameters to the routes (dynamic routing)

### Add the :parameter (router parameter)

Adding a colon and name creates a route parameter which can be accessed
by the component class.

```typescript
import { Routes } from "@angular/router";

const appRoutes: Routes = [
    // controller can retrieve the id parameter
    { path: 'users/:id', component: UserComponent},  // dynamic path
];
```

### Accessing the parameter in a component class

We can access the route parameter with ActivatedRouter object on the ngOnInit method (initialization of the component).

* We can access it by using the activatedRoute.snapshot.params;
* The template html can access it by refering the component's variables.


This is a nice approach to get the route params for the FIRST TIME the component is loaded!

```typescript
import { ActivatedRoute } from "@angular/router";

@Component({
    ...
})
export class Component implements OnInit {

    // component property that will receive the route parameter
    id : number;

    // inject the activated route to access the parameter
    constructor(private activatedRoute: ActivatedRoute) {}

    onMethod() {
        // redirects to the path RELATIVE to the actual route
        this.router.navigate(['relativePath'], {relativeTo: this.activatedRoute});
    }

    ngOnInit() {
        // retrives the id router parameter from the activatedRoute
        // excellent for the FIRST LOAD
        this.id = this.activatedRoute.snapshot.params["id"];
    }
}
```

* When we are on a component's page, angular does NOT instantiate it again, only if we navigate to another component and get back to it!

* Hence, by default, angular does not recreate a component! However, if the component's data change, we may want to reload!


### Listening to route parameters changes in a already loaded component

If the component was ALREADY loaded if the snapshot params, it will not reload again with new data if the route changes!

Observable data is excellent for handling async tasks! Listening to route changes is one!

Hence, we subscribe to the observable in order to eventually execute a callback when the async code is finished! We use this callback to UPDATE the component with the new route data"

Syntax:

```typescript 
    .subscribe(
        // first callback takes the Params object
        (params: Params) => {

        },
    );
```

This is essential when we are in the same component and want to update its data but Angular will not reload the ALREADY RELOADED component:

```typescript 
    ngOnInit() {
        // this updates the component as it is LOADED
        this.id = this.activatedRoute.snapshot.params["id"];      

        // this updates the component as the ROUTE CHANGES AFTER LOAD
        this.activatedRoute.params
            .subscribe(
                (params: Params) => {
                    this.id = params["id"];  // update component as route changes                
                }
            );  
    }
    
```

## <!> Important note <!> about subscriptions

When one subscribes to an observable, this subscription LIVES ON MEMORY even if the component is destroyed (Unless its an Angular-based observable);

Te safest way is to unsubscribe to custom observables is on OnDestroy!

```typescript 
    import { Subscription } from "rxjs/Subscription';

    ...

    subscription: Subscription;

    ...

    ngOnInit() {
        // this updates the component as it is LOADED
        this.id = this.activatedRoute.snapshot.params["id"];      

        // this updates the component as the ROUTE CHANGES AFTER LOAD
        this.subscription = this.something.subscribe(...);
    }

    ngOnDestroy() {
        // safely removes the subscription after component is detroyed!
        this.subscription.unsubscribe;
    }    
```

For angular-based subscriptions, unsubscribing is DONE AUTOMATICALLY by the framework and, therefore, not necessary! 

However, for CUSTOM subscription --> removing is ESSENTIAL to avoid unnecessary memory consumption!

## Query parameters

