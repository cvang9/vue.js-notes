### Here we discuss about Optimised Deployment Techniques :

1. Lazy Loading ||  Asyncronous Component Imports:

    Till now, we are following a method which states when user opens our application all the code were loaded to its browser containing all the components, irrespective of user is using that component or not.

    This might be heavy for big applications -> Can't we use an approach which can load components only when they are required?

    Vue Provides us exactly this thing - Import Components Asyncronously 

    SYNTAX:

{{

    import defineAsyncComponent from 'vue';


    // import CompOne from './components/CompOne.vue';
            //    |
            //    V

    const callback = () => {
        return './components/CompOne.vue';
    }
    const CompOne = defineAsyncComponent( callback );


}}

The process is called Lazy Loading...