# Q1 Explain what the simple List component does?
--> The simple list component is a React functional component that renders an unordered list of items. It consists of two sub-components: SingleListItem and List.

SingleListItem: is a memoized component that renders a single list item. It takes four props as input: index, isSelected, onClickHandler, and text. The index prop is a number that represents the position of the item in the list.

List: is also a memoized component that renders a list of items. It takes one prop as input: items. The items prop is an array of objects, where each object represents a single list item.

List uses the useState hook to keep track of the index of the currently selected item. It uses the useEffect hook to reset the selected index whenever the items prop changes. It also defines a handleClick function that is called when an item is clicked. This function updates the selected index state variable to the index of the clicked item.

The List component maps over the items prop and renders a SingleListItem for each item in the list. It passes the text, isSelected, and onClickHandler props to each SingleListItem. It also passes the index prop as the key prop to ensure each item is uniquely identified by React. When an item is clicked, the handleClick function is called with the index of the clicked item, which updates the selected index state variable. Finally, the isSelected prop is passed to each SingleListItem to indicate whether it is currently selected, which changes the background color of the item to green or red accordingly.

# Q2  What problems / warnings are there with code?
-->  Different problems / warnings we get are:

First error was we need to install the dependencies named 'props-types' by running 'npm i props-types'

Second error was in useState of 'setSelectedIndex' and 'selectedIndex' are given in reverse order. We need to reverse it like below given: const [selectedIndex, setSelectedIndex] = useState(null);

Third error was in this piece of code: WrappedListComponent.propTypes = { items: PropTypes.array(PropTypes.shapeOf({ text: PropTypes.string.isRequired, })), PropsTypes.arrayOf instead of PropsTypes.array and the third error was using PropsTypes.shape instead of PropsTypes.shape.

# Question 3: Please fix, optimize, and/or modify the component as much as you think is necessary.
-->  After removing errors and warning. The final result is:

import React, { useState, useEffect, memo } from "react"; import PropTypes from "prop-types";

// Single List Item const SingleListItem = memo(({ index, isSelected, onClickHandler, text }) => { const handleClick = () => { onClickHandler(index); };

return ( <li style={{ backgroundColor: isSelected ? "green" : "red" }} onClick={handleClick} > {text} ); });

SingleListItem.propTypes = { index: PropTypes.number.isRequired, isSelected: PropTypes.bool.isRequired, onClickHandler: PropTypes.func.isRequired, text: PropTypes.string.isRequired };

// List Component const List = memo(({ items }) => { const [selectedIndex, setSelectedIndex] = useState(null);

useEffect(() => { setSelectedIndex(null); }, [items]);

const handleClick = (index) => { setSelectedIndex(index); };

return ( <ul style={{ textAlign: "left" }}> {items.map((item, index) => ( <SingleListItem key={index} index={index} isSelected={selectedIndex === index} onClickHandler={handleClick} text={item.text} /> ))} ); });

List.propTypes = { items: PropTypes.arrayOf( PropTypes.shape({ text: PropTypes.string.isRequired }) ) };

List.defaultProps = { items: [ { text: "Item1" }, { text: "Item2" }, { text: "Item3" }, { text: "Item4" } ] };

export default List;
  
