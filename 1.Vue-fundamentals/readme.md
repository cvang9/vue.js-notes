
 1. Interpolation
 {{ something }} will print all the properties that was returned by data function in app.js this is called interpolation But the elements where we are using this must be Vue controlled.

2. methods property
By the help of {{}} ( interpolation ) we can execute basic javascript expression like function calling, ternary opertor, expression evaluation inside any html element.
By the help of methods we can simply execute a specific function when it will be required. 



3. Directives: Directives are basically an instruction to do something
a)  v-bind:attribute = 'data_property'
 What if we have to pass a dynamic data property to the attribute? then simple interpolation cannot be possible so there we use v-bind directive.

b) v-html = "data_property"
What if we want to put some dynamic html stuff inside an already existing html element? 
we simple can use v-html directive to instruct that the value must be treated as the html code 

c) v-on:event = "basic_javascript"
Whenever we wants to add an event listener we can simply use v-on directive followed by the event name.
By default this event pass an event argument, that describes all about the event which we use to find the input value.

d) v-on:submit = "basic_javascript"
If there's any case of submiting the form, there we can simply run our JS Code using this event handler.

e) `MODIFIERS`: When we have to add some special functionality or need to remove the default behaviour of the event then instead doing it manually we can simply use in-built modifiers.
{ ex: e.preventDefault() -> v-on:submit.prevent = "basic_javascript" }, here prevent is the modifier else we also can use traditional ways to do it like e.preventDefault()

f) v-once 
If we want to lock the initial value of any data binding or interpolation then we can use v-once directive. What it does is it only evaluates the expression just only once and hardcore it forever. 

g) Two way binding: ( v-model="data_property" )  ===  ( v-bind:value="data_property" + v-on:input="data_property" ); 
Why we are calling it two way binding because model is doing two things at the same time, like- It is taking input in a data_variable( v-on:input ) as well as at same time putting that value to the input box ( v-bind:value ).


TIP - Vue is super amazing beacuse of its high reactivity as we can see whenever any data variable updates then vue automatically detect the changes and re-renders them immediately. 
TIP- Also we can pass the parameters while we call our function in html atrribute.





4. Access Inside Data using this:
We are able to access the data properties using 'this' keyword just because the data object properties are added to global vue instance.
`Deep dive into the this keyword` : Basically it is the proxy object of the original Vue object instance that contains all the properties of data, methods and all those identifiers, it is created so that we can access only limited properties.






5. Disadvantage of using methods in html 
We know Vue is super intelligent, it automatically identifies the changes and re-render the modified code, however due to this Vue has to re-execute all the methods again on every render just because it don't know which method depends upon which data_property.
ex: we had changed counter, and suppose if any method that uses the counter it must be executed again. therfore Vue re-execute every html method on each render.

To resolve this issue here comes, computed-properties:
They are very essential and similar like methods-property, the only difference is this time Vue remembers the data_dependency which is used in the computed-function and only re-executes when that data_property is being modified which is used inside that computed-property.

so, when to use computed and when methods?
for simple interpolation don't use methods because it will re-executes instead use computed one because computed will remember all the dependency which it is acquiring.
for data-binding and data-events simply use methods.






6. We are using computed right? now imagine a data property 'a', now we need a specific function that must re-execute only when 'a' changes:
our options having computed, but in using computed what if it includes another property 'b' which we don't want to re-execute on change.
therefore we have another property for that :- `WATCHERS`
watch property allow us to executes set of statements on a changes of particular data_property.
( jesa naam vesa kaam : data_property ko watch hi krta rhega, ek threshold par kuch function krdega  ) :- ( ex: reset counter if its value reaches > 50 )
 



TIP) Shorthands for v-on: and v-bind: 
Since their usage is so common Vue provides us some shortcuts to code them. they are:- `v-on: => @` and `v-bind: => :`
ex: <button @click='buttonClicked' > click me</button> 
ex: <input :value="name" />



Here we will learn about a bit about components.

1. Component registeration
        so far we had seen components are registered in the main.js file.like: 

        const app = createApp(App);
        app.component('the-header', TheHeader);
        app.component('base-badge', BaseBadge);
        app.component('badge-list', BadgeList);
        app.component('user-info', UserInfo);

        
        also we know the fact that these all components not need in use for regular and global purpose. but by registering components in this type we face some issues:

        1. More load -> When page loads ,since all the components are initialised globally. Vue will make each component available from starting time itself, irrespective of the fact that some of components might not use regularly.
            To resolve this load we have an another approach of registering these components locally and put remain those components only which are required globally.

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