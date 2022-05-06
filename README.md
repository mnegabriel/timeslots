# Timeslots


This project is a study about drag & drop web technologies to create a intuitive way of creating a timetable of courses.

## Techlonogies used
- [Vue 2](https://v2.vuejs.org/)
- [Vue CLI](https://cli.vuejs.org/)
- [Vue.Draggable](https://github.com/SortableJS/Vue.Draggable)


## Components

### Drag

#### **DragProvider** ( [*link*](src/components/DragProvider.vue) )

Component created based on *Vue.Draggable*. It functions as a provider of courses, to be placed on the timetable. When a draggable element is placed in a receiver, the element is cloned to the receiver. 

##### Props

###### `groupName`

```js
{ type: String, default: '' }
```

The name of the group of *Vue.Draggable* components, necessary to indicate the connection between provider and receiver(s). It should be tha same af the respective receiver(s)


###### `value`

```js
{ type: Array, default: [] }
```

The array of data that the provider provides. Should be used in conjunction with `input` event. **It's preferable to use `v-model` instead of `value` prop and `input` event**

###### `draggableSelector`
```js
{ type: String, default: '' }
```

Css selector that indicates which elements inside the provider component should be draggable. If not specified, anything inside is draggable

###### `handleSelector`
```js
{ type: String, default: '' }
```

Css selector that indicates an element inside a Draggable element that will be used to drag the element itself. If not present, the element will be dragged wherever inside it is clicked.


##### Events

###### `start`

Emit when the drag movement is initiated. It propragates the *Vue.Draggable* event.
###### `end`

Emit when the drag movement is finalized. It propragates the *Vue.Draggable* event.
###### `input`

Emit when the array passed to it is changed by a *Vue.Draggable* action.



#### **DragReceiver** ( [*link*](src/components/DragReceiver.vue) )

Component created based on *Vue.Draggable*. It functions as a provider of courses, to be placed on the timetable. When a draggable element is placed in a receiver, the element is added to the receiver array. 
##### Props

###### `groupName`
```js
{ type: String, default: '' }
```

The name of the group of *Vue.Draggable* components, necessary to indicate the connection between the receiver with provider(s) and receiver(s). It should be tha same af the respective provider(s) and/or receiver(s)


###### `ableToRecieve`
```js
{ type: Boolean, default: true }
```

Boolean that control if the receiver can receive new elements

###### `value`
```js
{ type: Array, default: [] }
```

The array of data that the provider provides. Should be used in conjunction with `input` event. **It's preferable to use `v-model` instead of `value` prop and `input` event**

###### `handleSelector`
```js
{ type: String, default: '' }
```

Css selector that indicates an element inside a Draggable element that will be used to drag the element itself. If not present, the element will be dragged wherever inside it is clicked.


###### `ghostClass`
```js
{ type: String, default: '' }
```

Class to attach to the preview of the element that appears when hovered above it.


#### **DragDump** ( [*link*](src/components/DragDump.vue) )

Wrapper *Vue.Draggable* element that enables user to remove elements from a DragReceiver. When draggable elements are released in it, they won't be stored anywhere.

##### Props

###### `groupName`

```js
{ type: String, default: '' }
```

The name of the group of *Vue.Draggable* components, necessary to indicate from which draggable lists it will act upon.

### Course

#### **CourseBlock** ( [*link*](src/components/CourseBlock.vue) )

Component that represent a course, with multiple states: default, disabled, timeslot, ghost

##### Props 

###### `course`
```js
{ type: Object, default: {}, },
```

course data to be displayed

###### `draggableClass`
```js
{ type: String, default: '', },
```

Class to attach to component's main tag, to be linked with the draggable components behaviours. Required in case of this parent drag component has a dragabble-class

###### `handleClass`
```js
{ type: String, default: '', },
```

Class to attach to components handle element. If it is the same as the parent draggable component's handle-class, the element with `&` will be used to handle the dragging behaviour


###### `disabled`
```js
{ type: Boolean, default: false, },
```

Boolean that changes component to its disabled state as well as attempts to disable drag behaviour

###### `asTimeslot`
```js
{ type: Boolean, default: false, },
```
Boolean that changes component to its timeslot state.

#### **CourseSection** ( [*link*](src/components/CourseSection.vue) )

Component that separates courses into sections, purely a styling component that holds a list of courses

##### Named Slots

###### `courses`

```html
<template #courses="{ list }">
```




-------------------
## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn serve
```
