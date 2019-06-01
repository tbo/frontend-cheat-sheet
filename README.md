# Frontend Cheat Sheet

## Git

Pull changes from the remote repository:
```console
git pull
```
Change to a different branch (e.g. master):
```console
git checkout master
```
Create feature branch and change to it:
```console
git checkout -b features/SPH-42-Name_of_ticket
```
Get the names of all branches. The active one is highlighted:
```console
git branch -a
```
Commit changes:
```sh
git commit -am "SPH-42: Change description"
```
Stash changes:
```sh
git add --all
git stash
```
Get changes from stash:
```sh
git stash pop
```
## JavaScript

### Functions

Different function notations:
```typescript
function addOne(a) {
  return a + 1;
}

const addOne = (a) => {
  return a + 1;
};

const addOne = (a) => (
  a + 1
);
```
If arrow functions have **exactly one** argument and **no type annotations**, then the initial braces are optional. All of the following notations are valid:
```typescript
const addOne = (a) => (a + 1);
const addOne = (a) => a + 1;
const addOne = a => (a + 1);
const addOne = a => a + 1;
```
Functions can return as well:
```js
function add(a) {
  return function (b) {
    return a + b;
  }
}
```
Or shorter with arrow functions:
```js
const add = a => b => a + b;
```
Usage:
```js
add(1)(2) // returns 3

// Creates a function, that always adds 5
const add5 = add(5);

add5(3); // returns 8
```
### Destructing
```js
const address = {
  city: "Costa Mesa",
  state: "CA",
  zip: 92444
};
const {city, state, zip} = address;

console.log(city); // 'Costa Mesa'
console.log(state); // 'CA'
console.log(zip); // 92442

const nums = [1, 2, 3, 4, 5];

const [first, second,,,,fifth] = nums;
console.log(first, second, fifth); // 1, 2, 5
```
#### Alias
```js
const {city: c, state: s, zip: z} = address;

console.log(c, s, z); // 'Costa Mesa CA 92444'
```
#### Function Arguments
```js
const displayPerson = ({name, age}) => 
  console.log(`My name is ${name} and I'm ${age} years old.`);

const person = {name: 'Aaron', age: 35};

displayPerson(person);
```
### Spreading
```js
const nums = [1, 2, 3];
const abcs = ['a', 'b', 'c'];
const alphanum = ['x', ...nums, ...abs ]; // ['x', 1, 2, 3, 'a', 'b', 'c']
```
### Mapping
```js
const numbers = [2, 4, 8, 10];
const halves = numbers.map(x => x / 2);
// halves is [1, 2, 4, 5]
```
### Filtering
```js
const words = ["spray", "limit", "elite", "exuberant", "destruction", "present"];
const longWords = words.filter(word => word.length > 6);
// longWords is ["exuberant", "destruction", "present"]
```
### Reducing
```js
const total = [0, 1, 2, 3].reduce((sum, value) => sum + value, 1);
// total is 7
```

## React

### Components

Components can be implemented as stateless functions:
```jsx
import React from "react";

const GreetingComponent = (props) => (
  <span>Hello {props.name}!</span>
);

export default GreetingComponent;
```
```jsx
import React from "react";

const GreetingComponent = (props) => {
  const greeting = `Hello ${props.name}!`;
  return (
    <div>{greeting}</div>
  );
};

export default GreetingComponent;
```
Or as classes:
```jsx
import React from "react";

class NameComponet extends React.Component {
  render() {
    <div>Hello {this.props.name}!</div>
  }
}

export default GreetingComponent;
```
### JSX

#### Embedding Javascript
```html
<div>{Math.round(3.14)}</div>
```
This will result in:
```html
<div>3</div>
```
#### Passing props
```html
<GreetingComponent name="Sanduni"/>
```
This will result in:
```html
<span>Hello Sanduni!</span>
```
##### Strings
jsx offers shortcuts for certain data types:
```jsx
<CustomComponent foo={"Bar"}/>
```
can be written as:
```jsx
<CustomComponent foo="Bar"/>
```
##### Boolean
```jsx
<CustomComponent foo={true}/>
```
can be written as:
```jsx
<CustomComponent foo/>
```
#### Children

Nested jsx is passed as Prop:
```jsx
const GreetingSection = (props) => (
  <div>
    <h1>Greeting</h1>
    {props.children}
  </div>
)
```
This will result:
```jsx
<GreetingSection>
  <p>
    <GreetingComponent name="Sanduni"/>
  </p>
</GreetingSection>
```
Will result in this:
```html
<div>
  <h1>Greeting</h1>
  <p>
    <span>
      Hello Sanduni!
    </span>
  </p>
</div>
```
### State
```jsx
import React, { useState } from 'react';

const Example = () => {
  // Declare a new state variable, which we'll call "count"
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```
### Typed Components
```jsx
import React from "react";
import {Badge} from "styles";

interface Product {
  name: string;
  image: string;
  // Price in cents
  price: number;
}

interface ProductTileProps {
  product: Product;
  quantity: number;
  sale?: boolean;
}

const formatPrice = (price: number): string =>
  (price / 100).toFixed(2) + " €";

const ProductTile = (props: ProductTileProps) => {
  const productName = props.product.name;
  return (
    <div>
      <h1>{productName}</h1>
      <img src={props.product.image} alt={productName}/>
      <div>{formatPrice(props.product.price)}</div>
      <div>{props.quantity < 10 ? "Only few items left" : "Many items available"></div>
      {sale && <Badge>Sale!</Badge>}
    </div>
  );
};

export default ProductTile;
```
Usage:
```jsx
const exampleProduct = {
  name: "Nice shirt",
  image: "/images/nice-shirt.jpg",
  price: 3800
};

[...]

<ProductTile product={exampleProduct} quatity={42} sale/>

<ProductTile product={exampleProduct} quatity={9}/>
```
### Complex example
```jsx
import React from "react";
import {Badge} from "styles";
import formatPrice from "../utilities/format-price";

interface Product {
  name: string;
  image: string;
  price: number;
}

const ProductItem = ({name, image, price}: Product) => (
  <div>
    <h1>{productName}</h1>
    <img src={props.product.image} alt={productName}/>
    <div>{formatPrice(props.product.price)}</div>
  </div>
);

const ProductList = (props: {products: Product[]}) => {
  const productList = props.products
    // Only show products that are cheaper than 10 Euro
    .filter(product => product.price < 10000)
    // Sort by price
    .sort((a, b) => (a.price > b.price) ? 1 : -1);
  return (
    <div>
      <h1>Cheap Product List</h1>
      <div>Number of products: {props.products.length}</div>
      {productList.map(product => <ProductItem product={product}/>}
    </div>
  )
};

export default ProductList;
```
Usage:
```jsx
const products = [
  {name: "Blue shirt", image: "/images/blue-shirt.jpg", price: 9000},
  {name: "Red shirt", image: "/images/red-shirt.jpg", price: 13000},
  {name: "Green shirt", image: "/images/green-shirt.jpg", price: 8000},
]

const GenericComponent = () => (
  <ProductList products={products}/>
);
```
## Styles

### Styled Components
```ts
import {styled} from 'styles'

const Button = styled.button`
  color: red;
`
```
Usage:
```jsx
<Button>This my button component.</Button>
```
#### Passing props
```jsx
import {styled} from 'styles'

interface ButtonProps {
  primary?: boolean;
}

const Button = styled.button<ButtonProps>`
  color: ${(props: ButtonProps) => props.primary ? 'hotpink' : 'turquoise'};
`

interface ContainerProps {
  primary?: boolean;
}

const Container = styled.div<ContainerProps>((props: ContainerProps) => ({
  display: 'flex',
  flexDirection: props.column && 'column'
}))
```
Usage:
```jsx
<Container column>
  <Button>This is a regular button.</Button>
  <Button primary>This is a primary button.</Button>
</Container>
```
You can find more examples [here](https://emotion.sh/docs/styled)

