### REACT TEST SOLUTIONS
### Martin Mtandi
### Email: mtandimartin@gmail.com

### JAVASCRIPT
### 1. What is your favourite new javascript feature and why?
The spread operator. It allows us to create a copy of an array with so much ease. for example:
```
let array = [1, 2, 3];
let copyArray = [...array];
```
### 2. Explain an interesting way in which you have used this javascript feature.
The spread operator has made it easier for me to desctructor large objects or arrays. I use the spread operator a lot, especially when I am working with Formik. for example:

EXAMPLE 01
```
<Formik
    initialValues={{...INITIAL_VALUES}}
    validationSchema={FORM_VALIDATION}
    enableReinitialize
    onSubmit={values => {
        handleSubmit(values)
    }}
 >
    {({...rest}) => {
        console.log(rest.values);
        console.log(rest.errors);
        return (
            <Form>
               // your input fields go here
            </Form>
        )
    }}
 </Formik>
 ```

EXAMPLE 02
```
 const [state, setState] = React.useState({
    firstname: '',
    lastname: '',
 }}
 
 const handleChangeName = () => {
  setState({...state, firstname: 'Martin'})
 }
```
### 3. Is there any difference between regular function syntax and the shorter arrow function syntax? (Write the answer in your own words)
The difference between regular functions and arrow functions (sometimes refered to as fat arrow functions) goes beyond syntax. Fat arrow functions are particularly useful in functional based components in Reactjs especially when it comes to binding this. For example:
```
 function handleRegularFunction() {
  console.log(this);
 }

 const handleArrowFunction = () => {
  console.log(this)
 }

 handleRegularFunction();
 handleArrowFunction();
```
In the example above, the regular function will log the properties of the window. The arrow function will log properties of its parent.

### 4. What is the difference between ‘myFunctionCall(++foo)’   and  ‘myFunctionCall(foo++)’
++foo is a pre increment and foo++ is a post increment

### 5. In your own words, explain what a javascript ‘class’ is and how it differs from a function.
A javascipt class is a template for creating objects. Functions do specific things, classes are specific things. Classes often have methods, which are functions that are associated with a particular class, and do things associated with the thing that the class is - but if all you want is to do something, a function is all you need.

### CSS:

### 6. In your own words, explain css specificity.
In a case where there are 2 or more rules that apply to the same html element, the selector with the highest specificity value will be applied.

### 7. In your own words, explain, what is ‘!important’ in css.  Also how does it work?  Are there any special circumstances when using it, where it’s behaviour might not be what you expect?
!important is a css rule used to override any styling that might have been applied to that html element. Its behaviour might not be what you expect, especially when dealing with foreign CSS. Since !important contradicts the expected behaviour of CSS, its generally recommended to avoid using it.

### 8. What is your prefered layout system: inline-block, floating + clearing, flex, grid, other?  And why?
It really depends with what I am trying to achieve. I mainly use flex and grid. I use flex whenever I want my elements to align side by side without really paying attention to the width of each column. In a case where I want the width of each column to be the same I would normally use grid layout. 

### 9. Are negative margins legal and what do they do (margin: -20px)?
Its actually not a bad practice to have negative margins, as long as you're aware of the fact that you're using negative margins, and that this pulls/moves elements from their otherwise normal position.

### 10. If a div tag has no margin or other styling and a p tag inside of it has a margin top of some kind, the margin from the p tag will show up on the div instead (the margin will show above the div not inside of it), why is this?  What are the different things that can be done to prevent it?
The margin top of the child element also produces margins for the parent element. The reason for this problem is that the parent element does not have a complete inclusion, so that the child element cannot find the border or padding of the parent element; This is called 'margin collapsing'.
CSS stipulates:
The margin of all two or more adjacent box elements will be merged into a single margin share.
To solve this you can have an element inside the parent div, before your child element, which contains content (even &nbsp; should also prevent this).

### Unit tests:

### 11. What technologies do you use to unit test your react components?
Jest & React Testing Library

### 12. Are there any pitfalls associated with this technology that have caused you difficulty in the past?
None I can think of at this time.

### 13. How do you test in your unit tests to see if the correct properties are being passed to child components.

### PARENT COMPONENT

 import React from "react";
 import ChildComponent from "./ChildComponent";
 
 const ParentComponent = ({ open, data }) => (
  <>
    <p>Some content to render</p>
    {open && <ChildComponent open={open} data={data} />}
  </>
 );

export default ParentComponent;

### JEST TEST SCRIPT

 import React from "react";
 import { render } from "@testing-library/react";
 import ParentComponent from "./ParentComponent";
 const mockChildComponent = jest.fn();
 
 jest.mock("./ChildComponent", () => (props) => {
  mockChildComponent(props);
  return <mock-childComponent />;
 });
 
 test("If ParentComponent is passed open and has data, ChildComponent is called with prop open and data", () => {
  render(<ParentComponent open data="some data" />);
  expect(mockChildComponent).toHaveBeenCalledWith(
    expect.objectContaining({
      open: true,
      data: "some data",
    })
  );
 });

 test("If ParentComponent is not passed open, ChildComponent is not called", () => {
  render(<ParentComponent />);
  expect(mockChildComponent).not.toHaveBeenCalled();
 });

### REACT:

### 14. React test step1:

Create a react component that has a <div/> with a border.
Inside this <div/> should be a <span/> that displays the ‘live’ width of the browser window at all times.  Keep in mind that the size of the window could easily be changed by the user and you should reflect this.

 import React from 'react';

 export function App(props) {
   const [screenSize, getDimension] = React.useState({
    dynamicWidth: window.innerWidth,
  });

  const setDimension = () => {
    getDimension({
      dynamicWidth: window.innerWidth,
    })
  }
   React.useEffect(() => {
    window.addEventListener('resize', setDimension);
    
    return(() => {
        window.removeEventListener('resize', setDimension);
    })
  }, [screenSize])

  return (
    <div style={{border: '1px solid black'}}>
      <span>Live width: <strong>{screenSize.dynamicWidth}</strong></span>
    </div>
  );
 }

### 15. React test step2:

### Inside the <div/> you created in the previous step, add a text input that, as a number is entered into it, uses that number to set the height of the div itself in ### pixels, live as you update the text field (keypress not change event).

 import React, { useState, useEffect } from 'react'

 export default function UserWindow() {
  const [screenSize, getDimension] = useState({
    dynamicWidth: window.innerWidth,
    dynamicHeight: window.innerHeight
  });

  const setDimension = () => {
    getDimension({
        ...screenSize,
      dynamicWidth: window.innerWidth,
    })
  }
 
  console.log(screenSize)
  useEffect(() => {
    window.addEventListener('resize', setDimension);
   
    return(() => {
        window.removeEventListener('resize', setDimension);
    })
  }, [screenSize.dynamicWidth]);
  const handleKeyUp = (e) => {
    getDimension({
        ...screenSize,
        dynamicHeight: e.target.value,
    })
 }

  return (
    <div style={{border: '1px solid black'}}>
     <span>Live width: <strong>{screenSize.dynamicWidth} height: <strong>{screenSize.dynamicHeight}</strong></span>
     <br />
     <input name="dynamicHeight" defaultValue={screenSize.dynamicHeight} onKeyUp={handleKeyUp} />
   </div>
  )
 }

### 16. React test step3:

### Add the following code to your project root (same project as in step 2, but add the code in the global / window space):  

###    Let divHeight;
###    window.setDivHeight = (height) => divHeight = height;

### Add a HOC for your div component that allows you to set the height of your <div/> component from the previous steps by calling that external function.

### If you do not know what a HOC is or how to create one, that is also fine, just mention that in your answer and instead create a parent component that can still do ### this (allow you to call that function ‘setDivHeight’ in order to set the height of the div manually.

### Bare in mind that when the height of the div is forcefully set like this, the text fields value should also update to reflect this and should still carry on 
### working as normal (user can continue to modify its value).  


 import React from 'react';

 const HOC = (WrappedComponent) => {

    Let divHeight;
    window.setDivHeight = (height) => divHeight = height;

    return (props) => {
        return (
            <div style={{height: 'divHeight'}}>
                <WrappedComponent {...props} />
            </div>
        )
    }
 }
  
 export default HOC
