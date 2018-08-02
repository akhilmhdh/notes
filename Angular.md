# Angular Training

## Typescript

- Superset of JS
- Compiles to JS
- Static typed
- Type Errors at compile time

### Using Typescript

- Compiled using `tsc.exe`
- Need to install typescript

```bash
npm install -g typescript

tsc hello.ts
```

### Propertes

- Strongly typed.
  - Error if type of variable changes
- Type inference
- Type Annotation
- One variable can also contain multiple types.
  - `var s:string|number`

> Type `any` is a special typescript type which can be of any type. No strongly typing when using it.
>
> Implies:
>
> `v = 200; v = "hi"` is valid if v if of type any.
>
> Any variable declared without annotation or inference is of type any.

- Creating a JS Object variable results in a new type unless the variable is declared as any.
- All function parameters are required by default.
- Function param can be option using the syntax `function test(a:?int){}`
  - Optional params should always come after required.
- Functions can infer return type, but try to be explict.
- TS supports default params. `function test(a:int=0){}`
  - Should come after required.

#### Arrays

- `var obj:int[] = [1,2,3];`
- `var obj:Array<int> = new Array<int>(1,2,3);`

#### Keyword Let and const

- Let allows to create block scope variable.
- Const also block scope, but can't be changed.
  - Initialization should be along with declaration.
- Var creates function scope.

```typescript
function addition(x:number, y:number): number {
  let letTest:number =2;
  var varTest:number = 2;
  return x+y;
}
if(true){
  var test = "String";
}

console.log(test); // legal
console.log(letTest); //illegal
console.log(varTest): //illegal
```

#### For vs For-In vs For-of

- Typescript specific is `for-of`.
- For-in returns the index of current element.
- For-of returns the value of current element.
- Function ForEach

```typescript
//for-in
for( let car in cars){
  console.log(car)
} // Output 0,1,2
//for-of
for( let car of cars){
  console.log(car);
} // output Car1, Car2, Car3

cars.forEach(function(car, index) {
  console.log(car + index);
})
```

### String Templates

- Use backtics ( \` ).
- Can be used to interpolate values
- Can create multiline strings

E.g.

```typescript
var str:string = `Hello {a}

Hello`
```

### Arrow Function

- Similar to lambda function
- For annonymous functions

#### Syntax

```typescript
// Normal Function
function NormalSquare(x){
  return x*x;
}
//Function as an expression
var Square = function(x) {
  return x*x;
}
// Arrow functions
var NewSquare = (x) => {
  return x*x;
}
var NewSquare = x => {
  return x*x;
}
// Or
var NewSquare = x => x*x ;
```

#### Advantages

- Context binding happens at time of creation.

```javascript
function Emp(){
  this.a = 10;
  setTimeout(function(){
    console.log(this.a); // Will output undefinined
  })
}

function Emp(){
  this.a = 10;
  setTimeout(()=>{
    console.log(this.a); // Output 10;
  })
}
```

### Interfaces

- Used to define structure of an object/class.
- Any data members declares in interface should be present in the object implementing the interface.
- Data members can be options as well
- Can also contain declaration of function.
- Can extend other interfaces.

```typescript
interface ICompany {
  location:string;
  name:string;
  numberOfEmployees?:number;
  getDetails?():any;
}
```

### Classes

- Overloading syntax is complex hence not used.
- Constructor - `constructor(){}`

### Compiler Options

- `-w` :Watch for file changes. (Automatically recompiling in case of change in file)
- `tsc -h` for more options.

#### tsconfig.json

- File which contains compiler options.

`tsconfig.json`

## Introduction

### Why Angular

#### Single Page Application

- Less bandwidth
- More responive/interactive
- No need for full page refresh

### Node Package Manager

- `package.json`

Dev dependancies are packages needed while developing, e.g. to compile.

### Polyfills

Packages that can fill gaps in browser feature support.

## Getting started

### Components

- Used to emit out HTML
- Reusable
- Configurable
- Composible
- Maintainable

It is mandatory to mention **Selector** and **Template**.

```typescript
@Component({
  selector: 'my-app',
  template: `<h1>Hello {{name}}</h1>`,
})
export class AppComponent  { name = 'Angular'; }
```

Every component should be declared with one module such that the instances can be injected as and when needed.

- Any component declared in same module can directly use the selector of sibling components without needing any declaration.
- Each time a component (the HTML-selector) is used a new instance is created.

#### Passing values between components

- Passing data from parent to child using properties.
- Use `Input()` Decorator to be able to make a variable passable.
- Property binding syntax - used to bind a property with objects.

```typescript
<app-course [courseDetail]="c"></app-course>
```

What is passed within the property string is a typescript code. Any function call calls the function from the context of the instance. Any variable is accessed from it.

Similarly to pass an hardcoded string, encapulate it in single quotes. `<app-course [option]="'c'"></app-course>`

- Property binding ensures data is passed as-is without converting to string
- Property binding also possible with HTML properties.
- Ideally all property should be passed using the property binding syntax.

```typescript
<img [src]="product.ImageUrl">
```

### Enhanced Class Syntax

Instead of declaring each variable as it's own and assigning it from constructor. Enhanced class syntax is an easier way.

Instead of this:

```typescript
class Emp {
  name: string;

  constructor(name:String){
    this.name = name;
  }
}
```

You can use this:

```typescript
class Emp {
  constructor(public name:String){}
}
```

Both code are equivalent.

> This syntax works only is access specifier is written. Otherwise the variable is created as a local variable of the constructor.


## Angular Binding

- Process of attaching data.
- Multidirectional

From model to view -> One Way.

> You should never manupulate DOM yourself.

Binding says, changing model will automaticaly update the view it's bound to.

One-way binding:

- `{{data}}` -> Interpolation
- `<.. [value]="data">` -> Property Binding

**Two Way Binding**: Reflecting changes in UI into the model object. This is done using `ngModel` directive.

> Need to `import {FormsModule} from @angular/forms` in order to use `ngModel` which is two way binding.

### Importing Modules

You need to mention the dependent modules in `imports` of **NgModule**.

All of the imports and declarations should be put in `NgModule`.

## Structural Directives

These are used to manupulate the structure of the HTML
E.g.

- `*ngFor`: A for-of loop inside HTML template
- `*ngIf`: For conditional rendering od template

```typescript
@Component({
  selector: 'app-product',
  template: `<h2>{{product.Name}}</h2>
  <input type="checkbox" [(ngModel)]="isFree" />
  <h4 *ngIf="!isFree"><b>Price: </b>{{product.Price}}</h4>
  `
})
export class ProductComponent {
  @Input()
  public product:Product

  isFree: boolean = false
}
```

The above code segment shows working of `*ngIf`. If the checkbox is checked, the price will not be shown.

## Binding Styles

Done using `ngClass`.

This expects either:

- A string property from context

  ```typescript
  <div [ngClass]="productClass">
  ```

  Product class must be a variable in component class.
- An array of strings

  ```typescript
  <div [ngClass]="['productClass','highlight']"
  ```

- An Object:

  ```typescript
  <div [ngClass]="{'productClass':true,'highlight':false}"
  ```

  The boolean value does not need to be hardcoded but can come from model.

## Pipes

These are operators used to transform any text while text interpolation.

e.g.

```typescript
{{price | currency}}
```

This will print the value of price as a currency with symbol and all.

### Writing custom pipes

- Pipes are implemented as classes.
- Decorate a class with `@Pipe` to make it a pipe.
  - Pass parameters like name for it there.
- Implement `PipeTransform`.

e.g.

```typescript
@Pipe({
  name:"qty"
})
export class QuantityPipe implements PipeTransform {

  transform(value: number) {
    if (value > 0)
      return value;
    return "Out Of Stock";
  }

}
```

> Make sure to declare the pipe in your module, in order to use it.

- You can make pipe accept parameters like

```typescript
{{ data|pipe:arg1:arg2:}}

@Pipe(..)
export class TestPipe implements PipeTransform {
  transform(value:any, arg1:any, arg2:any, arg3:any){
    ....
  }
}
```

The parmeter can also be array or an object.

E.g.

```typescript
{{ data| pipe:{val:false} }}

@Pipe(..)
export class TestPipe implements PipeTransform {
  transform(value:any, obj:any){
    //obj is the object passes as param
  }
}
```

### Safe Navigation Operator

Can be used to interpolate object properties which may be underfined. `{{product.name}}` Will give an error if product is undefined. `{{product?.name}}` Will not throw an error in case product is undefined.

## Forms

Forms can be used to interact with users.

- Create a form in template using the form tag.
- Each input in form should have a name to keep track of state.
- Each input should be bound to a model (better if a property of a model object).

### Getting Angular Objects for HTML elements

Use `#<id>="<class>"` to obtain the object.

E.g.

```html
<form #f="ngForm">
<input type="text" #iname="ngModel""/>
```

These objects can then be used within the template to access angular variables etc.

### Form states

- Valid or Invalid
- Pristine(untouched) or dirty(changed value)

These are added as classes to form inputs and form.

- These states can be used to perform actions programically, like displaying error.

e.g.

```html
<form #f="ngForm" (ngSubmit)="addNewProduct()" class="form-inline">
  <p class="error" *ngIf="iname.dirty && iname.invalid">
    Product name required!
  </p>
  <input type="text" #iname="ngModel"name="pname"  class="form-control" [(ngModel)]="newProduct.Name"     required placeholder="Name"/>
  <p class="error" *ngIf="ipric.dirty && ipric.invalid">
      Product Price required!
    </p>
  <input type="text" #ipric="ngModel"name="pprice" class="form-control" [(ngModel)]="newProduct.Price"    required placeholder="Price"/>
  <p class="error" *ngIf="iquty.dirty && iquty.invalid">
      Product Quantity required!
    </p>
  <input type="text" #iquty="ngModel"name="pqty"   class="form-control" [(ngModel)]="newProduct.Quantity" required placeholder="Quantity"/>
  <input type="submit"
  [disabled]="f.invalid"
  value="Add Product"
  class="btn btn-success"/>
</form>
```

## Switch

Switches can be used to implement if-else chains.
It is created as:

```html
<ul [ngSwitch]="searchQuery">
  <li *ngSwitchCase="a">a</li>
  <li *ngSwitchCase="b">b</li>
  <li *ngSwitchDefault>Default</li>
</div>
```

When `searchQuery` is equal to 'a'. It will show first element. Otherwise the default.

## Services

- Used to share business logic accross components.
- Used to share data accross components.

They are simply classes whos instances will be used by components.

Always provided as dependency, using dependency injection.

Required services needs to be declared as dependency so that angular can inject it when the time comes. This is done by using the value providers.

```typescript
@Component({
  selector: 'app-sample',
  templateUrl: './app/SampleComponent.template.html',
  providers: [SampleService]
})
export class SampleComponetn {..}
```

Next step is telling angular that the component needs an object of it's dependancy. This is done by declaring it in the constructor (Called constructor level injection).

```typescript
constructor(private _sampleSerivice: SampleService){ ... }
```

> Note that is example uses enhanced class syntax. The same can be re-written as
>
> ```typescript
> constructor(sampleService:SampleService){
>   this._sampleService = sampleService
> }
> ```

Now the component will have an object of the required class (**service**).

- `@Injectible` tells angular that a service uses another service, and it is fine to inject services.

### Scope of services

- If the service is declared in component, i.e. while `providers` is mentioned in `@Component` decorator. Then every new instance of component will have new instance of service created.
- If the service is declared in module i.e. `@NgModule`, then same instance will be used accross components using the service.
- Importing a module will also import the dependencies and services along with it.

## Making Network Requests

### For Angular 2.4x

Network requests are made using HttpModule. Using `Http` service.

Create a service to provide the required data. Give it the dependency of `Http` and pass it via constructor injection.

Use `Http.get` etc. to make the network calls.

This returns an observable. Which can be further returned from the service to the component.

### Observable Collection

These are data objects which contains values that may change. We can subscribe to the change.

Use the `subscribe` method to observe for any changes and update accordingly.

E.g.

```typescript
_postService.getPosts().subscribe(response => {
  this.postList = response.json();
});
```

> This is part of the **Publisher Subscriber** pattern

### Promise

A promise for a data in a later date. It can either **resolve** or gets **failed**.

> Promise Object is an ES6 feature. Not always supported. Instead rxjs is used.

States of promise:

- Pending
- Fulfiled
- Rejected

Use the function `then` to provide functions to be called then it **fulfiles** or when it **fails**.

E.g.

```typescript
_postService.getPosts().then(response => {
  this.postList = response.json();
  console.log("it worked!");
},
err => console.log(`Error occurred!: ${err}`));
```

#### RxJs

Open Source library which adds methods to objects in runtime.

### When to use observable vs Promises

- Promises are used for one-time usage, fetching data in one go.
- Observable are used when the collection may change e.g. more data may arrive.

Essentially, promises once fulfiled will end. But in case of observable, if the server pushes more data again, that kind of situations observable is useful.

## Life Cycle Hooks

There are functions called by angular during different parts of it's lifecycle called lifecycle hooks. These allows dveloper to put business logic into corresponding places. E.g. to initialize a component before it's rendered `ngOnInit` can be used instead of puting it in the constructor.

It's good practice to implement the interface defining the lifeccle functions.

E.g.

```typescript
export class PostComponent implements OnInit{

  constructor(private _postService:PostService) {
    // only code nessesary to create the object.
  }

  public ngOnInit(): void {
    // Initialize the component.
  }
}
```

Find more at [documentation](https://angular.io/guide/lifecycle-hooks).

## Routing

- Client side routing.
- Everything happens on client side.

### Use routing

- Import `RouterModule`
- Create an object of `Routes` (which is basically `Route[]`).

```typescript
const routes:Routes = [
  {
    path: 'posts',
    component: PostComponent
  },
  {
    path: '',
    component: ShoppingCartComponent
  }
]
```

- Point the router module to follow the set of routes.

```typescript
@NgModule({
  ...
  imports:      [ ..., RouterModule.forRoot(routes), ... ],
  ...
})
```

- Set base url in the index page.

```html
<html>
  <head>
    ...
    <base href="/">
    ...
  </head>
  ...
</html>
```

- Use the router outlet in a component (**Root Component in most cases**).

```html
...
<router-outlet></router-outlet>
...
```

To navigate using the defined routes, using `href` with the anchor tag `<a>` will refresh the page. Instead use `routerLink` directive.

```html
<a routerLink="<route>"></a>
```

> Routes can have child routes as well, which will be used to obtain something like `<route>/<child-route>`.

### Parameterized routes

Paths can contain parameters which can be used e.g. `/posts/<id>`

This is done using the syntax:

```typescript
{
    path: 'posts/:id',
    component: PostDetailComponent
  },
```

### Getting details of current path

Declare a dependency of `ActivatedRoute` object.

```typescript
constructor(private _currentRoute: ActivatedRoute) {}
```

Use this object to get params and other details.

```typescript
this._currentRoute.params.subscribe(p => {
  let data = p[<key>]
});
```

## Angular 5 with CLI

### Install CLI

`npm install @angular/cli -g`

### Angular 5 vs 2.4

- Use of webpack
- `HttpClient` from `@angular/common/http` instead of Http service.
- Else section for `ngIf`.

```html
<div>
  <ul *ngIf="courses.length >0; else NoCourseTemplate">
    <li *ngFor="let c of courses">{{c}}</li>
  </ul>
</div>

<ng-template #NoCourseTemplate>
  <h6>No Course available yet</h6>
</ng-template>
```
