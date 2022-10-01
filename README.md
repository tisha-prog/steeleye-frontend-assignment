# steeleye-frontend-assignment

## Q-1.  Explain what the simple List component does.
   _______________________________________________________________________________________________________________________________________________________________________
Ans-  The list component renders a bulleted list of items which can be passed as props to the list component in the form of an array which consists of objects
like, text and key and it also changes the colour when clicked, from red to green.
The memo component is another component which will cause react to skip rendering of the list component if the props are not changed.

## Q-2. What problems / warnings are there with code?

_______________________________________________________________________________________________________________________________________________________________________

Ans -
#### 1. The syntax for useState is wrong
The correct way is this ---> 

  `const [selectedIndex, setSelectedIndex] = useState();`
  
#### 2. The onClickHandler used in WrappedSingleListItem needs to be used in a arrow function because onClickHandler is taking an argument
The correct way is this ---> 
   `   onClick={() => onClickHandler(index)} `
 
and not

This --------->
` onClick={onClickHandler(index)}  `

#### 3. The argument passed to setSelectedIndex inside the useEffect arrow function cannot be null as it needs to be a boolean value.

 ` useEffect(() => { setSelectedIndex(false); }, [items]);  `
 
 #### 4. The default values of defaultProps of WrappedListComponent cannot be null as the propTypes contains isRequired constraint and it also does needs values to map to the items, it cannot map through null items

` WrappedListComponent.defaultProps = { items: [ { text: "William", key: 1 }, { text: "Soumya", key: 2 }, ], }; `

#### 5. The propTypes should be used with arrayOf instead of array & shape should be used instead of shapeOf

` WrappedListComponent.propTypes = { items: PropTypes.array(PropTypes.shapeOf({ text: PropTypes.string.isRequired, })), }; `

## Q-3. Please fix, optimize, and/or modify the component as much as you think is necessary.

_______________________________________________________________________________________________________________________________________________________________________

Ans - 

``` import React, { useState, useEffect, memo } from "react";
import PropTypes from "prop-types";

// Single List Item
const WrappedSingleListItem = ({ index, isSelected, onClickHandler, text }) => {
return (
<li
style={{ backgroundColor: isSelected ? "green" : "red" }}
onClick={() => onClickHandler(index)}>
{text});
};

WrappedSingleListItem.propTypes = {
index: PropTypes.number,
isSelected: PropTypes.bool,
onClickHandler: PropTypes.func.isRequired,
text: PropTypes.string.isRequired,
};

const SingleListItem = memo(WrappedSingleListItem);

// List Component
const WrappedListComponent = ({ items }) => {
const [selectedIndex, setSelectedIndex] = useState();

useEffect(() => {
setSelectedIndex(false);
}, [items]);

const handleClick = (index) => {
setSelectedIndex(true);
};

return (
<ul style={{ textAlign: "left" }}>
{items.map((item, index) => (
<SingleListItem
onClickHandler={() => handleClick(index)}
text={item.text}
index={index}
isSelected={selectedIndex} />
))});

};

WrappedListComponent.propTypes = {
items: PropTypes.arrayOf(
PropTypes.shape({
text: PropTypes.string.isRequired,
})),
};

WrappedListComponent.defaultProps = {
items: [
{ text: "Tisha", key: 1 },
{ text: "Singh", key: 2 }, ],
};

const List = memo(WrappedListComponent);

export default List;  
```
