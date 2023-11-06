#                                              P I N I A :::  New Way Of State Management

What is Pinia? 
It is a library used to manage the states in Vue.js



Why use Pinia over Vuex?
After introducing Vue3 dev community is now shifting towards Pinia from vuex just beacuse it offers more features than vuex:
1. Simplicity
2. Dev Tool Support
3. Typescript Integration
...


When use Pinia?
When we need such type of data which must be available in multiple components irrespective of thier relationship, there we create a Pinia Store.


// Pinia store can be created using two config. (a) Options Store  (b) Setup Store, we go with setup store first beacuse it is simple than Options one.

# Step 0.  `npm i pinia` 


# Step 1.  Create Pinia Instance
{{
    ...
    import { createPinia } from 'pinia';

    const pinia = createPinia();
    const app = createApp(App);

    app.use(pinia);

    app.mount('#app')
    
}}



# Step 2.  Define Setup Store

Using function `defineStore( param1, param2 )`

param1 => A unique name given to store for entire application.
param2 => Can pass two things -
                (a): Option Object for creating Options Store
                (b): Setup Function for creating Setup Store


Directory Convention: Put each store in a different file.
Naming Convention:    The name must be like ->  use_anything_Store.   ex: useCounterStore().


// store/counter.js
{{
    import { defineStore } from 'pinia';
    import { ref, computed } from 'vue';

    export const useCounterStore = defineStore( 'counter', function(){

        // state
        const count = ref(1);

        // getters
        const doubleCount = computed( function() {
            return count.value*2;
        });

        // actions
        function increament(){
            count.value++;
        }

        return { count, doubleCount, increament }
    } )
}}




# Step 3. Use in Component

{{

    <template>
        <h1> {{ counterState.count }} </h1>
        <h1> {{ counterState.doubleCount }} </h1>
        <button v-on:click="counterState.increament"> Increase </button>
    </template>
    

    <script>
    import { useCounterStore } from '../store/counter.js';

    // For Destructuring
    // import { storeToRefs } from 'pinia';

    setup: function(){
        const counterState = useCounterStore();

        // const { count, doubleCount } = storeToRefs(counterState);
        // const { increament } = counterState;


        return { counterState }

        // return {
        //     count,
        //     doubleCount,
        //     increament
        // }
    }
    </script>


}}


#    Destructure states 
// Want to destructure items from useCounterStore to avoid doing  state.property ?
Yes you can but will face some reactivity issue, therefore we have `storeToRefs()` using this we can destructre as we want and still we will not face any reactivity issues.

ex: 
{{
    <template>
        <h1> {{ count }} </h1>
        <h1> {{ doubleCount }} </h1>
        <button v-on:click="increament"> Increase </button>
    </template>
    

    <script>
    import { useCounterStore } from '../store/counter.js';
    import { storeToRefs } from 'pinia';

    setup: function(){
        const counterState = useCounterStore();

        const{ count, doubleCount } = storeToRefs(counterState);
        const { increament } = counterState;

        // Methods can directly be destructured because we not need a reactive function.

        return { count,
                 doubleCount,
                 increament 
                }
    }
    </script>


}}






#        Mutate Data If needed using `$patch()`
Whenever we need to update state or create a new state, then do not do directly because it may lose reactivity instead use path function.
It expects function in its parameter, and in the callback do whaterver mutation you wanna do..

ex: 

{{

    setup: function(){

        const store = useCounterStore();

         // Patch method to mutate states
        store.$patch( function(state){


           // NO need to use .value suffix if state is a ref or reactive 

            state.name = 'shivang';
            state.age = 20
            state.skills.push('vue.js');
        });

    }
}}

