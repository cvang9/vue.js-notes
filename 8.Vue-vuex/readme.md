### Let's Rock With a new State Management Of Vue : VUEX        
#     `npm install --save vuex@next`

   Q- What is state? 
   A- State is nothing but the Reactive Data.

    We already have Provide and Inject then why we need to come up with another solution?
    Provide and Inject grinds well but in large scale applications devs observe some issues in it 
    like

 1.   Components become fatty  : Consisting huge data and logic in single component.
 2.   Unpredictable Reactivity : Sometimes Provide-inject does unpredictable issues due to which its become hard to debug
 3.   Error-Prone              : For some reason data can be modified.


    All problems has specific solutions but all ends up with one destination as a Best Solution i.e. VUEX





##  SETUP: following the pattern 

{{   // main.js

    import { createStore } from 'vuex';

    const store = createStore( {

        state: function(){
            return {
                counter: 0,
            }
        }
    }) 

    app.use(store);}}

    Vuex provides us a STORE ROOM (store) where we can store our reactive data( states ) globally.
    It means we can access this store from anywhere in the app.
    [ We can create many stores, but one store is assigned to one app ]


    Accessing the store state from any component of the app:
#    `this.$store.state.counter`





###  We can change the state from any where in the app. Sounds crazy Right?
     so technically and obv. it is not the preferable method to change our states form anywhere. Reason?

  1. If we have to do modification in state then we go to all that places and change it directly.
  2. if anywhere typo happened in modification, then Debugging will not be Easy.


  To solve this issue here comes mutations: 
  In this process we directly not change the state but write the logic in mutation and call that mutation when needed.
  that makes our code more usable and safe from typo.

#  USING MUTATIONS: 
{{
  const store = createStore( {

    store: function({
        return{  value: 0 }
    }),

    mutations: {
        increament: function(state){
            state.value += 2;
        }
    }
  })

}}

#  CALL MUTATIONS

    {{this.$store.commit('increament')}}


##   Passing Params to Mutations:   
    As you might be suggesting Can we pass data to mutations? YES.
    But where Can we pass?
    Yeas you think it right. IN the second argument of commit


    this.$store.commit( 'fxn_name', any_data_type ); 

    //Whatever you pass here as it is automatically will passed to mutation's second parameter.

    fxn_name: function( store, val ){ }





###   All Okay for the states where we have to modify them. but wait... don't you think this same problem will occur while reading the data?

    How?  suppose reading + manipulating

    Consider a scenerio: We need a value of counter after multiplied with 2.

    every time we interpolate state :{{ this.$store.state.counter*3 }}  and like there i made a typo of 3. Anybody can do the same. also if it is written in multiple places then we have to change from all of there.


    Again we need a getter function!, VUE provides us exactly {{getter}} property to do so. (ACCESS state from 1 place 

{{
    const store = createStore( {

        state: function({
            return{
                counter: 0
            }
        }),

        mutations: {},

#                                   The 2nd param allows us to fetch any getters method
        getters: {

            getCounter: function( state, getters ){
                return state.counter*2;
            },


            getNormalisedCounter: function( state, getters ){

                if( getters.getCounter > 100 ){
                    return 100;
                }

                else return getters.getCounter;
            }

        }
    })
}}

 #  ACCESS THE GETTERS METHODS IN COMPONENTS  
    {{ this.$store.getters.getCounter }}








###  Now Vue Devs got stucked in running async code in mutations. 

    What actually the problem is happening what if a mutation has a async code and while first mutation call is executing mean while 2nd triggered and executed -> this leads to unexpected and false results. 
    therefore, vue devs disables to execute async code inside mutations.

    Problem-  Where to execute Async code?
    Solution- We have {{ actions }}

    Actions acts in between component and mutations -> Allows all type of code async/sync to execute and only move forward once one is completed.

{{
    Good Practice : Always use actions between component and mutations irrespective of sync/async code
    Syntax:

    const store = createStore({

        state: function(){
            return{
                counter: 0
            }
        },

        mutations:{
            increament: function( state, payload ){
                state.counter += payload;
            }
        },

        actions: {
            // Accepts two params 1. context => needful to call mutations , 2. payload => data which is passed by component
            increase: function( context, payload ){                   
                context.commit('increament', payload );
            },
        }

    })



  => Call from Components:
    this.$store.dispatch('increase');

}}



/// Power Of context parameter: 

    context param has access of almost everything we had studied in vuex so far.

   1. context.dispatch()
   2. context.commit()
   3. context.getters.prop
   4. context.state

   You know why action holds all of the stuff?  that's because action are made because of this only. They are built to do functions and after a certain steps do mutations.





###    Shorthands of accessing  this.$store.getters.VAL   &   this.$store.dispstch( 'action_name', {} )


// Accessing this.$store.getters.VAL
{{
    
 <template>
     <h1> {{ getter_name }} </h1>
 </template>

<script> 

    import { mapGetters } from 'vuex';

    export default {

        computed: {
            // return this.$store.getters.getter_name
            ...mapGetters( ['getter_name'] );
        }
    }

}}    


// Accessing this.$store.dispatch( 'action_name' , {}  );

{{

    <template>
        <button v-on:click="action_name( {value: 1} ) " > Perform Action </button>
    </template>

    <script>
        import { mapActions } from 'vuex';

        export default {

            methods: {
                ...mapActions( [ 'action_name' ] )
            }
        }
    </script>
}}










###  Using Modules to organise your Code structure

    suppose take all the code related to counter and put it into another object, but it must be structured in the same pattern like it is written in the Store Object.

    Now use 'module' property of Store Object and put counterObject inside it.

{{

    const counterObject = {

        state: function(){
            return {}
        },

        mutations:{},

        getters: {},

        actions: {}
    }


    const store = createStore({

        modules: {
            counter: counterObject
        }

        // ....
    })


    // After all they all are merged together in all lies in the store instance.
}}



###   Namespace ->  You can seperate your local storeObject from root store by setting its namespace property: true

      Why do we do this, In bigger projects there might have clashes between the names of two or more stores. because at the end of the day they all merged together into a single store.
      therefore we can use namespace property to seperate our module with root level.
      But in future we can access the properties using assigned namespaces only.

{{
      const obj = {
        namespace: true,
      }

      const store = createStore({

        modules: {
            number: obj,                   // Namespace name is 'number'
        }
      })



      ACCESS_MECHANISM:
      You have to use an identifier `number` to every place where we are using that obj.

      this.$store.getters['number/getter_name'];

      this.$store.dispatch( 'number/action_name', {data: 'huehuehue'} );

      ...mapGetters( 'numbers', ['getter_name'] );
      
      ...mapActions( 'numbers', ['action_name'] );


}}


####  PEACE OUT




