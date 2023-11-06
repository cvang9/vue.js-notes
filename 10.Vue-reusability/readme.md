
###   ---------------------------     I M P R O V E        R E U S A B I L I T Y     ---------------------------------

When go to complex projects we definitely see a lot components accross a app using a same logic, then we may think can we use a same piece of code again and again? Well Using vue YES, let see how?

# OPTIONS API      -     Mixin
# COMPOSITION API  -     Custom Composition Functions


# ______________________________________________ MIXIN


1. UNDERSTANDING `MIXIN 's`

Let say we have two components AddUser and DeleteUser where both has exact same logic for OptionsAPI, there we can use MIXIN:  Create a new file under Mixin directory name of your choice and import all of the js code from component to here.

Now import that mixin and use :

{{

    <script>

    import userMixin from '../mixin/userMixin.js';
    import component1 from './components/component1.js'

    export default {

        components:{
            component1
        },

        // Use in-built mixin property of OptionsAPI
        mixins: [ userMixin ],
    }

    </script>
}}

real ex: AddOne, SubOne component in App.js

# REMARKS :

We can't import and register component in the mixin file, therefore we have to put the component registration seperately inside component <script> tag only.


What If : we add mixins as well as other properties also like data, computed, methods?
-> Then Vue by default merges all the data together correspondingly. eventually all will be part of a single object.


What If : Mixin and Traditional props define variables with same name?
-> Then Vue overrides mixin data variable with Original Props data.  In this case Default properties Wins.



# Global Mixin' ->
                   If somehow we can apply mixin to every component rendered then it is called Global Mixin.
                   We can do so by applying mixin at the Root level.

{{

    import { createApp } from 'vue';
    import myMixin from './mixin/myMixin.js'

    const app = createApp({});

    app.mixin(myMixin);

    app.mount('#app');
}}








## REAL DISADVANTAGE OF USING MIXIN'    

-> If we have a large project then we might struggle in finding the appropriate function because mixin must not be only one, they can be multiples and its hectic to find just one variable in those 6 files.






# ______________________________________________ userDefined Hooks

Obv. We use pure javascript functions here to re-use our code. that functions are named as `Hooks`;

Refer performActionHook.js inside hooks folder

# REMARKS

-> Must return an array in hooks for better readibility.
-> Hooks must not import components.
-> Components using hooks must destructure variables using appropriate name.






