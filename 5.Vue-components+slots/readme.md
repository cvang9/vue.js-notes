
v-once => `npm install -g @vue/cli`


       =>  `vue create project_name`

___________________________________________________________________________________________________________________________________

IMPORTANT NOTES:


Here will see about the components, but first let see why we need components actually.

-> Suppose we have to render the same things again and again, but for this problem we simply can use array of objects and render them using v-for directive.
but the problem occurs when For each object we wanna use a particluar button that does a specific function.
if we create using methods it will work for others objects as well at the same time.

To solve this issue we have Components.
Components are encapsulated and isolated peice of data that is used for reusability of same code.
Encapsulated and Isolated: Components also have its own properties ( data, computed, methods ) and they have no scope outside the components, also components cannot access its main app properties, therefore there's no name clash if both having variables of same name. 

SYNTAX: app.component( 'comp-name', {  data, computed, template, methods  } );


_____________________________________________________________________________________________________________________________________

Multiple Vue Apps vs Multiple Components

You might recall lecture 3 ("Different Ways of Using Vue"): You can use Vue.js to control parts of (possibly multiple HTML) pages OR you use it to build so-called "Single Page Applications" (SPAs).

If you control multiple, independent parts of HTML pages, you will often work with multiple Vue apps (i.e. you create multiple apps by calling createApp() more than once).

On the other hand, if you're building a SPA, you typically work with just one "root app" (i.e. createApp() is only used once in your entire codebase) and you instead build up a user interface with multiple components.

You absolutely are allowed to also use components in cases where you have multiple Vue apps but you typically won't use multiple Vue apps if you build one big connected user interface.

Why?

Because Vue apps are independent from each other - they can't really communicate with each other. You might find "hacks" to make it work but there's no great "official" way of sharing data between apps, updating something in app A in case something happens in app B etc.

Components on the other hand - as you will learn soon - DO offer certain communication mechanisms that allow you to exchange data between them. Hence you can build one connected UI if you work with one root app that holds multiple components.

You'll see that in the lectures and throughout the entire course, especially in the course projects of course!




Title = COMPONENTS

1. We can pass props as an html attribute in any order but it must follow the kebab-case naming convention, Vue itself compare this kebab-case attributes with camelCase props properties.
2. Vue only allows uniderection data flow that having some protocols. like 
  
   Problem a)-> You can't modify value of a recieved prop. ( At the recieved junction(component) it will act as mutated )
   Solution  -> Well if our app needs to change the value then we have two patterns of doing that.

          Pattern a.1) It says change the value of prop but first aware this modification to the parent first and he will do this all. ( Value is changed in both sides )
          Pattern a.2) It says just copy the value of prop and do change according to you. ( data is changed only for child component )   


          Solving pattern a.1)-

          Since Vue allows only unidirectional flow of data from parent to child we have to use another approach to send data from child to parent, one of the solution is Event-Driven-Programming
          Whenever we have to change any prop, just emit an event using -------->  ` this.$emit( 'event-name', data  ) `.
          // For Composition API we can use context.emit(), context is a 2nd param of setup function.
          Now at the parent side apply an event-listener at the component attribute named ' @event-name="fxn_name" ', inside fxn_name(){  // do whatever changes has been done }


          TIP)- If you're using any event in your component then good practice will be if you put that event into emits object inside properties. It will be awesome if you add some validators also.
          ex: emits: {
                 'toogle-bff': function( val ) {  return (val)? true: false  }
                     }          


3. Validating The props - Vue allow us to validate the props we are accepting from the parent is valid or not. It gives us various properties to check the validity like:
       
        a) type: String | Number | null | boolean | undefined | NaN etc...
       
        b) required: boolean 
       
        c) default: any_value              ( if not required then what should be the default value )
       
        d) validator: function(value){     ( if it returns true then only it will accept the prop else throw an error )
            return (boolean_logic) 
        }

        Example: 

        props: {

            name: {
                type: String,
                required: true
            },
            contactNumber: {
                type: String,
                required: true
            },
            emailAddress: {
                type: String,
                required: true
            },
            bff:{
                type: String,
                required: false,
                default: "0",
                validator: function( val ){
                    return ( val === "1" || val === "0" );
                }
            }

        }                               
   


4. :bind Props in while passing the values so that data can be dynamic.



5. Fallthrough in Vue.js ( also called attribute inheritance )
    Suppose a Component : component.vue 
__________

    <template>  
        <button>
            <slot></slot>
        </button>
    </template>
 
    <script>export default {}</script>
____________

   Now its parent Component is calling in such a way: 

__________

    <component-name type="submit" @click="doSomething">Click me</component-name>

___________

its final render is :

------------

        <button type="submit"  @click="doSomething">
            Click me <slot></slot>
        </button>

-------------

It is because of Fallthrough, it states that if the component will not register the props and emits in its properties then it will be automatically applied with the root element of the component's template.





6. Binding All Props on a Component

if we are transferring all the key-value pairs of an object via props, then simply on shorthand we can send directly that object.

ex:

_______________________________________________________________________________________ 
<template>
  <user-data :firstname="person.firstname" :lastname="person.lastname"></user-data>
</template>
 
<script>
  export default {
    data() {
      return {                                                                                        
        person: { firstname: 'Max', lastname: 'Schwarz' }
      };
    }
  }
</script>

_______________________________________________________________________________________

                                                    |
                                                    |
                                                    |
                                                    |
                                                    |
                                                    v

_______________________________________________________________________________________
<template>
  <user-data v-bind="person" ></user-data>
</template>
 
<script>
  export default {
    data() {
      return {                                                                                          
        person: { firstname: 'Max', lastname: 'Schwarz' }
      };
    }
  }
</script>
_______________________________________________________________________________________








7. Again we stuck in a problem that what if we need to pass data from 1st parent to 6th(nth) child, then do we follow linear pattern of passing props like: 1 -> 2 -> 3 -> 4 -> 5 -> 6 which is called pass through.
Definitely not we use another approach provided by Vue.Js. i.e. { Provide and inject }
We can provide the data in the 1st parent and inject it directly in 6th child.  

SYNTAX: 
        1st Parent:
            
        export default {

            provide: {
                return {
                    variable_name: this.any_data_property,
                }
            },

            .....
        }

        // For Composition API we import { provide } from 'vue'; and use in setup function:

        provide( 'unique-name', payload );



        6th child

        export default {

            inject: [ 'variable_name' ],

            .....
        }

        // For Composition API we import { inject } from 'vue'; and use in setup function:

        const data = inject( 'unique-name' );


This is how we can avoid pass through, although pass through is not a bad approach to pass props.
NOTE:- Flow of provide-inject can only be occured in parent-child relationships.






8. Like we are sending props from 1st parent to 6th child. Can we do this similarly in case of events ? like 6th child is calling the event and the function that should be executed will be of 1st parent one. and all of this happen without passthrough.
    Yeah Definitely Vue have a solution for this, it is simple just provide a function of 1st parent in provide property and inject the function where it is required( 6th Child ).

    ex: 

    1st Parent: 

    export default {

        methods: {
            func(){
                clg('huehuehue')
            }
        },

        provide: function(){
            return{
                fn: this.func,
            }
        }
    }


    6th Child:

    <template>
        <button  v-on:click="fn" />
    </template>

    <script>
        export default{
            inject: [ 'fn' ]
        }
    </script>

Now Whenever button is clicked then always func() is called.











9. Here we will learn about a bit about components.

1. Component registeration
        so far we had seen components are registered in the main.js file.like: 

        const app = createApp(App);
        app.component('the-header', TheHeader);
        app.component('base-badge', BaseBadge);
        app.component('badge-list', BadgeList);
        app.component('user-info', UserInfo);

        
        also we know the fact that these all components not need in use for regular and global purpose. but by registering components in this type we face some issues:

        1. More load -> When page loads ,since all the components are initialised globally. Vue will make each component available from starting time itself, irrespective of the fact that some of components might not use regularly.
            To resolve this load we have an another approach of registering these components locally and make remain those components only which are required globally.

            A component can locally registered in this way:


            import ComponentA from './';
            import ComponentD from './';

            export default {

                components: {
                    'component-name': ComponentA       ||   componentName : ComponentA
                    'component-name': ComponentD     
                }
            } 

        
        now in this way, at the very starting vue will not load unneccesary components instead load when they are needed.


10. Scoped Styling 

    We know the fact that whatever style we apply inside <style> tag is applied globally in a component file. Question is Is Vue provide us any feature to apply styles just specific to component only.
    Obv. If we  are talking about this, then you know the answer must be YES ! good guess,

    Vue provide us scoped styling that does the same which we wants.

    SYNTAX: 

    <style scoped >                   scoped attr allow to make css specific to components, it is made possible by assigning a unique attribute to each component elements.
    </style> 




11. Slots

    Slot is used to replace some dynamic HTML from parent app to child-component app.
    
    <div>
    <component>                                   <template>                                          
        <h2> HUEHUEHUE </h2>          +             <slot></slot>                ==>           <h2> HUEHUEHUE </h2>
    </component>                                  </template>                                  
    </div>

      Parent.vue                                 ChildComp.vue                        rendered code of component file

      That simply states, slots are used to replace some dynamic HTML from parent App;



12. Named slots

In a single component there can be multiple slot exists. To configure them we just give name to all of these.

ex: 

<template>

    <div>
      <slot name="first"> </slot>
    <div>
    <slot name="second"> </slot>

</template>


Use the slots using v-slot:name_of_slot directive in the template tag.
ex: 

<template>
    <section>
        <h1> HEMLO! </h1>
        <slot-component>
            <template v-slot:first>
                <h2> Killer </h2>
            </template>
            <template v-slot:second><h2>CYANS</h2></template>
        </slot-component>
   </section>
</template>


If we don't give name to the last slot, its name is automatically assigned with 'default';







13. Under the Hood, How slot works? SIMPLE !

What Vue does is, it first evaluates the whole parent html template and then it sents the required markup to the child component-slot. 
so there will proper effect of all the things( properties, styles etc... ) we had studied so far in vue.




14. More About Slots

14.a)  Suppose for any reason parent file do not pass the HTML data to the slot, then what will happen? 
       Vue provides us an extra fxnality : DEFAULT Value in slots.

        <slot name="first" >
        <h1> Default Value </h1>           
        </slot> 

        This time if parent do not send content to "first" slot then it will Display  --  Default Value


14.b) we can access the incoming slots value by - this.$slots

    A new property :
    ex: mounted: function() {
        console.log(this.$slots)
    }

    from this we can simply identify which slot is coming from the parent.

    // In Composition API we can access with `context.slots`, again context is the 2nd param of setup function.







15. Moving towards the advance features:

    Scoped Slots:
             You might be wondering what is scoped slots, well to understand this assume a situation where you might need to pass some data from slots to your parent.

             ex: 
               <li v-for=" arr in array" > 
                     <slot> </slot>                                    ->    This parent template must need {{ arr }} to display the things.
                </li>
            
            To resolve this type of situations, Vue has another cool feature to pass props via slots.

            ex: 
               <li v-for=" arr in array" >                                                                                   <template v-slot:default = "slot_props" >
                     <slot v-bind:item = "arr" > </slot>          ->    Now the parent template can acces it as ->             <h1> {{ slot_props.item }} </h1>
              </li>                                                                                                          </template>
                
            
    NOTE: The props recieved by parent is in the form of object and can only be accessed with the same name of the prop we had passed into the slot.
    Can see the code of SlotList Component.







16. Making Components Tab

    Think of the devil and devil is here  =>   click on the component's button and component is here

    Suppose we are having two components: 'active-user' and 'inactive-user', but we wants such a UI that, only render one whose button is clicked.
    for this purpose Vue also provide us another cool feature. 

    <component  :is="component_name" >  </component>                ->  What Vue will do is whichever component_name we had passed will be rendered in place of this <component/>

    and we can make this component_name dynamic by using `v-on:click` event-listener.

    ex: 

    <button v-on:click="setActive" > Active-use </button> 
    <button v-on:click="setInActive" > InActive-use </button>  

    // Assume setActive and setInActive methods toggles component_name in between 'active-user' and 'inactive-user' respectively'
    ex: Refer the Code in App.Vue -> FirstComponent and SecondComponent




17. BTS how Dynamic component works>
    Actually when another button is clicked reffering another component, then current component is removed from the DOM itself and new Component is added immediately.

    Problem: What if we had some data left in previous component? Ex: <input /> ( only half input is given and for some reason we have to move into another component )
    -> Vue also provides solution to this problem, giving us 
    
    <keep-alive>
        <component :is="comp-name" > </component> 
    </keep-alive>

    It ensures the input which is left off must be exist if the component is toggled.







18. TELEPORTATION 

    Yeah! Vue provides us also a feature of teleport html elements from anywhere to anywhere, 
    Sometimes we encountered a situation that we have to place and evaluate the elements here only but should render on different component.
    There we can simply use <teleport to="css_selector" > </teleport>

    <teleport to='body'>
        <dialog>
          <h1> {{ ERROR }} </h1>
        </dialog>
    </teleport>

    Now this dialog will be shifted to the body end of the body element.




### In this module we'll learn about CSS transitions

  0. <transition>
  0.      <slot> </slot>
  0. </transition>


->  When we wrap any element then, vue gives us 6 new already applied classes through which by using them we 
    can  animate that element in our app. 

    The classes are:   
    
    .v-enter-from {}                                              .v-leave-from {}
    .v-enter-active {}                                            .v-leave-active {}
    .v-enter-to {}                                                .v-leave-to {}



###   Transition Namings

  0. <transition name="myclass" > 
  0.   <html-element></html-element>
  0. </transition>

    .v-enter-from {}                  ->                          .myclass-enter-from {}
    .v-enter-active {}                ->                          .myclass-enter-active {}
    .v-enter-to {}                    ->                          .myclass-enter-to {}






