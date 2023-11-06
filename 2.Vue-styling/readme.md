here we disscuss the basic dynamic styling properties.

1. Style property 
Simple use javascript expression inside the style object to use classes dynamically.
ex-
:style="{ backgroundColor: (1>0)? 'black' : 'white' }"


2. Special Classes functionality to make styles dynamic
using v-bind directive we can instruct class whether to add a class or not by giving boolean parameters to it.

ex:     v-bind:class="{ className: boolean_data_property }"
if  boolean_data_property  is truly then class will be applied else it will not.

3. We can apply classes on element by passing array of 'classnames'.
ex: :class=" [ 'class1', { class2: boolean} , 'class3' ] " 

