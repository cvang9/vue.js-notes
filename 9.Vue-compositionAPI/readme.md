###  C O M P O S I T I O N      A P I


What and Why ?

Till now we are building our apps using Options API. 

{{

    // OPTIONS API
export default {

    data: function(){

    },

    computed: {

    },

    methods: {

    }
    
}
}}


All things were working great... till users dived in creating complex apps.
Problem occurs when after successfull creation we see our code un-organised + de-fragmanted based on logic.
i.e. Same logic data, methods are not grouped but distributed accross the code.


Composition API help us to achieve the same thing. Group the same logic code and also improves Code Reusability.
`Visit Articles for more`




# look out the Composition API working !

All of the properties are replaced here {{  "data", "computed", "methods", "watch"   =>    "setup"  }}

{{
    export default {

        setup: function(){
            //....
        }
    }
}}



# How Can we create reactive variables then?

Good question >>.  
A- Using ref       :   ref allows us to create reactive variables.

{{

    <template>
        <h1> {{ value }} </h1>
    </template>

    <script>

        import { ref } from 'vue';

        export default {

            setup: function(){
                const val = ref('initial_value');

                // Must return am object through which our Dom can access these reactive data so far.
                return { value: val }
            }

        }

    </script>
}}

likewise Options API, we can't access any props or reactive data using `this` keyword, because it is initatised before component initialisation.


## SYNTAX SUGARING :::      script with setup()    =>    <script setup ></script>

{{
    <template>
        <h1> {{ val }} </h1>
    </template>

    <script setup >

        import { ref } from 'vue';
        
        const val = ref('initial_value');

    </script>
}}



// IMPORTANT :  `ref` might behave differently with objects. let see

{{

    setup: function(){

        const val = ref( { name: 'shivang' } );

        setTimeout( ()=>{
            val.value.name = 'shiva';
        } , 2000 );
        
        return { val: { name: val.value.name }  }            // =>   return { val : val }
    }
}}

In this case your output will not change, its simple because the object which you had returned does'nt contains any reactive variable (its just a normal string) .
But in the newer syntax this issue is automatically resolved, cuz we don't have to return object anymore. 
Actually we have to return the reactive variable only to see the reactivity but here we are returning a simple variable.


# even though we also have a solution for this => `reactive`

Use `reactive` instead of `ref`;
Difference ? reactive works for only objects and it makes a proxy object to whichever we had passed to it.
so don't need to use  `var_name.value.property` instead directly use  `var_name.property`




# REMARKS

Check if variable is a ref?                             `isRef( var-name )`
Check if object is a ref?                               `isReactive( object-name )`
Convert all properties of a reactive object to refs?    `toRefs( reactive-object-name )`
Destructure values from a ref object                    `storeToRefs( object )`






### Coming Up to the topic [    Way to create methods in ComAPI    ]

let see how can we create methods in this way of making component: Don't you think its easy? yeas

{{
    <template>
      <h1> {{ name }} </h1>
      <button v-on:click="setName"> Change Name </button> 
    <template>

    <script setup>
      
      import { ref } from 'vue';

      const name = 'Shivang';

      function setName(){
        name.value = "shiva";
      }

    </script>

    // Simple as that
}}






### Coming Up to the topic [    Way to create computed in ComAPI    ]


Vue provide us a specific function `computed` that takes a callback in its parameter where we return our computed logic.

{{

    <template>
        <input v-on:input='setFirstName' /> 
        <input v-on:input='setLastName' /> 

        <h1> {{ fullName }} </h1>
    </template>

    <script setup>
        import { ref, computed } from 'vue';

        const firstName = ref('');
        const lastName  = ref('');

        function setFirstName(e){
            firstName.value = e.target.value
        }

        function setLastName(e){
            lastName.value = e.target.value
        }

        const fullName = computed( function(){
            return firstName + ' ' + lastName;
        });
    </script>



}}









### Coming Up to the topic [    Way to create watch in ComAPI    ]


Watch is also a interesting property from Options API which we simply import as an function from 'vue'.
It takes two params 
  1. Dependency Variable or Dependency Array          
  2. A callback( newValue, oldValue )

SHOW ME THE CODE:

{{
    <script setup>
        import { ref, watch } from 'vue';

        const age = ref(0);
        const year = ref(2000);

       // Dependecy Variable
        watch( age, function( newAge, oldAge){
            ...
        });


        // Dependency Array
        watch( [age, year], function( newValuesArray, oldValuesArray ){
            ...
        })

    </script>

}}











####          TEMPLATE REFS ::: DOM_ref


<input  ref='myName'  />            =>         can be accessible in optionsAPI using  `this.$refs.myName`

Query     - but `this` do not exists in CompositionAPI, what to do now?
Resoloved - Can use another approach of accessing refered element using ( .value )

{{
    <template>
        <input ref="nameElement" />
        <button v-on:click="setName" > Update Name </button>
    </template>

    <script setup >

        import { ref } from 'vue';

        const nameElement = ref(null);
        const name = ref('')

        function setName(){
            name.value = nameElement.value.value; 
        }

    </script>
}}





# _________  WE CAN MIX      C O M P O S I T I O N   A P I       AND      O P T I O N S    A P I        _________________




###       ACCESS `this` keyword is not allowed,  How can we access props, slot names then?

Setup method by default carry two parameters with it, the very first one is props means we can access props.

ex: 
{{
    <script>
         
         export default{
            setup: function( props ){
                console.log( "This is the props that this component accepts : " + props );

                return { }
            }
         }

    <script>
}}


The second param setup will got is `context`.

Context contains three things :

1. >    attrs      -> contains all the fall-through properties    
2. >    slots      -> contains all the slot details if component do have slot
3. >    emit       -> Can used to emit an event using context.emit( 'event-name', payload );

# SYNTAX :

{{

    export default {

        setup: function( _, context ){

            context.attrs._prop1;
            context.slots._prop2;
            context.emit( 'event-name', payload )

        }
    }
}}













##         Now I am stucked in Provide and Inject.  

Simple ! import { provide } from 'vue';
Simple ! import { inject } from  'vue';

{{
    // Provider
    import { ref, provide } from 'vue';

    export default {
        setup: function(){
            const value = ref(10);
            provide( 'pro-val', value );
        }
    }


    _________________________________________________

    //Inhaler
    import { inject } from 'vue';

    export default {

        setup: function(){
            const val = inject('pro-val');

            export { val: val };
        }
    }

}}



# Now I am stucked in Routing...to change my route using $this.route.push() or fetching out some query using this.$route.params.id

 =:> vue-router gives us all in one solution for all of this by giving a `hook`: `useRouter`

 {{
    import { useRouter } from 'vue-router';
    export default {
        setup: function(){
            const route = useRouter();
            // redirect anywhere
            route.push('/product');
            // Get Params
            route.params.pid;
            // Get Query Params
            route.query.sort;
        }
    }
 }}





# Now I am stucked in State Management [ vuex ]...  to disapatch ACTIONS or get GETTERS i need `this.$store`. 

=:> For dealing with these vuex provide us aother hook called `useStore`

{{
    
    import { useStore } from 'vuex';

    export default {

        setup: function(){

            const store = useStore();

            // Get Getters
            store.getters.getterName;

            // Dispatch Actions
            store.dispatch( 'Action-name', payload );
            
        }
    }
}}






###                 _______________   LIFE-CYCLE METHODS IN COMPOSITION API ________________

# REMARKS
->  All are imported from 'vue';
->  All can be utilised inside `setup()`
->  All recieves a callback in thier param which executes when they are triggered. 


1. beforeCreated and Created  :

We actually do not need of beforeCreated | created because setup Methods executes at the same time. 
Whaterver beforeCreated and Created can do can also be done by setup()


2. beforeMount  and  mounted  :

They are changed -> add 'on' before the name and here's are new methods:
`onBeforeMount()` &  `onMounted`


3. Similarly with -> beforeUpdate,       updated,         beforeUnmount,        unmounted
                  `onbeforeUpdate()`  `onUpdated()`    `onBeforeMounted()`   `onunMounted`