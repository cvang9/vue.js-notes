
Talking About the Refs:
Vue also allows us to store the DOM elements internally and use them whenever its needed.

ex: let use input element as a reference
<input  :refs="any_unique_value"  />

now inside app.js we can access the element using `this.$refs.any_unique_value`.  ( if we log `this.$refs.any_unique_value` then o/p:  <input refs="any_unique_value"  /> )
const value = this.$refs.any_unique_value.value;


For Composition API 
use 2nd approach to get value : `any_unique_value.value`  




______________________________________________________________________________________________________________________________________________

VUE.JS (BTS) :-

When any data property changes, Vue do not directly update it to the actual Browser DOM.
before it goes through the Virtual DOM.

let's look out the story ->
Suppose a  property is modified, now what Vue does is re-creates a new Virtual DOM with the help of previous Virtual DOM and compare both of them. After finding the differences between them it updates those modifications to the final Browser DOM.
that's how a scene is created.

Virtual DOM looks like  { el : 'h2' , child: 'huehuehue' }, so it becomes easier to compare and modify the contents of virtual DOM.


________________________________________________________________________________________________________________________________________________


-> Various phases of Instance Lifecycle:

1. beforeCreate()     ->   triggers before initialisation of all the properties.
2. created()          ->   triggers after all the properties are initialised. ( data, methods, computed etc ) 

      |
      |
      | (Vue code is transpiled into pure html)
      |
      V

3. beforeMount()      ->   triggers before updating the code to actual DOM.
4. mounted()          ->   triggers after when all code is mounted to actual DOM. ( from here we can now see the Vue upgraded UI )
   
      |
      | (data_property modified)
      |
      V

5. beforeUpdate()     ->   triggers if any changes happen to any data_property. Executes before changes will display to UI.
6. updated()          ->   triggers if any changes happen to any data_property. Executes after changes will display to UI.

      
      |
      | ( Unmount the instance from actual DOM ) 
      |
      V

7. beforeUnmount()    ->   triggers if user forcefully unmount the already mounted application instance. Executes before unmount takes place.
8. unmounted()        ->   triggers if user forcefully unmount the already mounted application instance. Executes after unmount takes place.

