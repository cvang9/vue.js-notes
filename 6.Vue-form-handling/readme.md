
 VUE x FORMS 

-> lets see how Vue deals with forms:-
   
   1. Input mechanisms:
   In vue we are having many ways of taking input like: `v-on:input, v-model, ref`
   The question is which one is very useful ?
   the question cannot be answered straightfully because one is apple while another is mango.

   despite finding best from both, lets see the features and use them accordingly in our application.

   1.a) v-model : <input type='number'  v-model='data_property' />
        In this case v-model automatically type cast the input to number type.

   1.b) ref :     <input type='number' ref='ip' />
        In this case whatever we get from `this.$refs.ip.value` is not a number type is by default a string data type.
    
    
   1.c) v-model with dropdown : <select v-model="data_property" >
                                   <option value="val" > Value </option>  
                                </select>
        v-model works same with select-option dropdown also. But by default we have to pass a value to data_property for default selection. ( will show empty option when we enter any value that does'nt exists )
    

    1.d) v-model works same in `checkboxes` and `radio buttons`.
        -> For the checkboxes the data property must be initialised as an array, but if there is a single checkbox
           then it will be a boolean type variable.
        -> Also assign value attribute to each checkbox if they are multiple.

        => For radio-buttons there is nothing something special.
    

    
    2. Form live Validation
    We can use our previously learned directives to validate the input fields.
    TIP : use `v-on:blur` event listener to trigger an event if user ignore first input and go for below ones.
    Then just play with styling....


    3. Adding v-model functionality on user-defined components
    Yes Vue enables us to leverage v-model upon custom components.
    but HOW? basically v-model is the combination of two bindings:  (a) `v-on:input`,(b) `v-bind:value`
    also we can make these properties for us also. that's how Vue allow us to use v-model on custom components

    How to use ? 
    <form>
    ....
    <component-name v-model="data_property" > </component-name>
    </form>


    Component Definition:
    <template>
    <button v-on:click="any_fxn('first')" v-bind:class= { active: "modelValue === first" } > FIRST_VALUE </button>
    <button v-on:click="any_fxn('second')" v-bind:class= { active: "modelValue === second" } > SECOND_VALUE </button>
    <button v-on:click="any_fxn('third')" v-bind:class= { active: "modelValue === third" } > THIRD_VALUE </button>
    </template>

    <script>

        export default {

            props: [ 'modelValue' ],

            emits: [ 'update:modelValue' ],

            methods: {

                any_fxn: function( val ){
                    this.$emit( 'update:modelValue', val );
                }

            }
        }
    </script>

    <style>

        .active = {
            border-color: black;
        }
    
    </style> 

    
