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