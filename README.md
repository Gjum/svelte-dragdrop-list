# DragDropList

This component renders a list of items which can be reordered by dragging and dropping, and uses the `flip` animation for adding/removing/reordering items in the list.

### Basic Example

```jsx
<script>
  import DragDropList from 'svelte-dragdrop-list';

  let list = ["First Item", "Second Item", "Third Item"];
</script>

<DragDropList bind:list={list} />
```

## Props and Slot

| name   | type      | required | default |
| ------ | --------- |:--------:| ------- |
| `list` | Array     | ✔️       | ✖️ |
| `key`  | Function  | ❌       | `item => item` |
| `slot` | Component | ❌       | `<p>{key(item)}</p>` |

You are required to pass a `list` prop to the component, for example by using `bind:list={myList}` for two-way binding,
or with `list={myList} on:reordered={onReordered}` if you need to do more event handling.
The list must be an array and may contain anything, but if the array contains objects or arrays you must pass the `key` prop to specify what property is going to be used as key (needs to be unique to each object in the array).

You can customize what element is used as the list item passing any element as the child.
If you do this you can access the item data by setting `let:item` on `<SortableList>` and `{item}` on your element.
You can also access the index by using `let:index`.

## Events

The component handles all the internal functionality but since you are passing the list as a prop, it can't actually change the data you pass to it, so you need to respond to an event that gets triggered any time you sort items.
This is done using the `on:sort` event handler, which gets passed an `event` object that contains the list inside the `details` property ( this is the default way of handling event data in svelte).


### Complete Example

```jsx
<script>
import DragDropList from 'svelte-dragdrop-list';
import Component from './Component.svelte';

let myList = [
  {id: 1, name: 'First Item'},
  {id: 2, name: 'Second Item'},
  {id: 3, name: 'Third Item'}
];
function onReordered(ev) {
  myList = ev.detail;
  console.log("List was reordered");
}
</script>

<DragDropList 
  list={myList} 
  key={item => item.id}
  on:reordered={onReordered}
  let:item
  let:index
>
  <Component {index} {item} />
</DragDropList>
```
