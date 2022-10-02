# Steel-Eye-Assignment
Steel Eye Assignment

Question 1.) Explain what the simple List component does.
<br>
Solution 1.) The List Components display unordered lists with texts as items. Using WrappedListComponent, we can create an
unordered list. It accepts an array (items) of strings (texts) as props. It maps through the array and displays the
content for each item on the screen with the color 'Red' as its background. When a user clicks on an item, it gets selected
and appears green background.
<br>
<hr>
Question 2.) What problems/warnings are there with the code?
<br>
Solution 2.)
a) Unique key prop is missing for List items.<br>

         <b>
         <pre> 
         <ul style={{ textAlign: 'left' }}>
            {items.map((item, index) => (
              <SingleListItem
               onClickHandler={() => handleClick(index)}
               text={item.text}
               index={index}
               isSelected={selectedIndex}
                />
            ))}
            </pre>
            </b>
<br>
b) Function reference instead of a function call on *onClick* event.
<br>
        <b>
        <li style={{ backgroundColor: isSelected ? "green" : "red" }}
     onClick={onClickHandler(index)}>
      {text}
</li></b>
          <br>
c) useState variables is not placed at correct position<br>
       <b> <pre> 
        const [setSelectedIndex, selectedIndex] = useState();
       </pre></b><br>
         
d) In mainstream we don't like defining props as NULL<br>
        <b> <pre> 
        WrappedListComponent.defaultProps = {
          items: null,
          };
          </pre>
          </b>
          <br>
e) Syntax Errors<br>
       <b> <pre> 
       WrappedListComponent.propTypes = {
                items: PropTypes.array(
                PropTypes.shapeOf({
                    text: PropTypes.string.isRequired,
               })
               ),
          };
          </pre></b><br>
f) Passing a number selectedIndex to isSelected<br>
         <b> <pre>
         <ul style={{ textAlign: "left" }}>
          {items.map((item, index) => (
          <SingleListItem
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex}
          />
          ))}
          </pre></b><br>
          
<br>
<hr>
Question 3.) Please fix, optimize, and/or modify the component as much as you think is necessary.
<br>
Solution 3.)
<b>
<pre>
import React, { useState, useEffect, memo } from 'react';
import PropTypes from 'prop-types';

// Single List Item
const WrappedSingleListItem = ({
index,
isSelected,
onClickHandler,
text,
}) => {
return (
<li
style={{ backgroundColor: isSelected ? 'green' : 'red' }}
onClick={() => onClickHandler(index)}
>
{text}
);
};
WrappedSingleListItem.propTypes = {
index: PropTypes.number,
isSelected: PropTypes.bool,
onClickHandler: PropTypes.func.isRequired,
text: PropTypes.string.isRequired,
};
const SingleListItem = memo(WrappedSingleListItem);

// List Component
const WrappedListComponent = ({ items, }) => {
const [selectedIndex, setSelectedIndex] = useState();
useEffect(() => {
setSelectedIndex(null);
}, [items]);
      const handleClick = index => {
      setSelectedIndex(index);
   };
   return (
        <ul style={{ textAlign: 'left' }}>
             { items.map((item, index) => (
                     <SingleListItem
                            key={index}
                            onClickHandler={() => handleClick(index)}
                            text={item.text}
                            index={index}
                           isSelected={Boolean(selectedIndex)}
                       />
                    ))}
             </ul>
            )
   };
  WrappedListComponent.propTypes = {
          items: PropTypes.arrayOf(PropTypes.shape({
                  text: PropTypes.string.isRequired,
         }).isRequired
        ).isRequired
   };
  WrappedListComponent.defaultProps = {
           items: undefined 
  };
const List = memo(WrappedListComponent);
export default List;`
</pre>
</b>
