#  Lets learn Routing from Scratch

   => `what` is routing?
       Routing is a process which allow developers to load different contents on a page using thier specific URLs. 
    
   => `Why` need routing?
       Whenever i have to share a specific component from a SPA then without routing it will display its default component because it is rendered first in main.js file. 

       but what if we have to share a specific component only? 
       traditionally we can create another html file and make its seperate route. After all sent it!
       but again it will no longer behaving as a component of our SPA. 

       to resolve this issue routing comes to save us.
       In vue it gives us a package through which we can create specific routes for specific components.
    

### npm install --save vue-router


#  SETTING UP vue-router

    import { createRouter, createWebHistory } from 'vue-router';
    
    const router = {

        history: createWebHistory(),
        routes: [  
             {path:'localhost:8080/users', component: importedComponentName }, 
             // On which path the component should render
        ]

    }

#    app.use( router );


    we had defined what will rendered on which URL, but till now not discuss where to render. for this just replace: 
#   <component-name> </component-name>   ->   <router-view>  </router-view>


    How to create a anchor tag which don't reload. don't need vue-router already give us this super-power
#   <a href='/user' >Users</a>             =>       <router-link  to="/users"> Users || <h1>hue</h1>  </router-link> 
    router-link under the hood using (<a>) tag and gives us a slot for defining some html.    


#   `a.router-link-active`      -> this class will be applied on clicking <router-link> or <a> (reference)
     there is also another class called `router-link-exact-active`, is applied only when the route exactly matches with to atrribute value.
    ex: /user/id         -> partially match
    ex: /user            -> exactly match




#    NAVIGATE PROGRAMATICALLY   using  this.$router

    Upon completion of any task if you forcefully wanted to navigate to any other route, then vue allow you to do this using this.$router

    SYNTAX ->  this.$router.push('/user'); 
    We have many methods like this to perform manual routing.  [ ex: `.forward()`, `.back()` to move forth and back ] 

    // For Composition API use -> useRouter() hook/
    import { useRouter } from 'vue-router';

    const router = useRouter();
    router.push();
    router.forward();
    router.backward();



#    Add parameters in path dynamically and Access them

    const router = createRouter({
        history: createWebHistory(),
        route: [
            { path: '/team/:teamId', component: Comp_Name }
        ]
    });

#   `Inside the component we can access this teamId dynamically`: 'this.$route.params.teamId'   ||   router.params.teamId
     Now do whatever you wanna do with this dynamic teamId.




###  DISCUSSING THE MOST ADVANCE FEATURE OF VUE.JS

# `typically whenever component is rendered Vue immediately load and creates it to the DOM but when we discard it Vue itself stores the component in the browser cache for better optimisation ` ? why -> `because in future if it will be rendered again then instead of re-create, Vue re-use the cached component`.

    Well here this can create a bug for us, 

    What if on residing the same component if we render it with a different URL. since in URL props will be same so Vue can't identify that it is called with different URL, so it will render the same content again but, the $route variable will update itself.

    therefore to resolve this issue, we have multiple approaches:

#   1st: Assign a watcher to `$route`  so that whenever `$route` change it will caught by watcher and do again the render.
 




###  CAN WE PASS THOSE URL PARAMETER IN THE COMPONENT AS PROPS ???
     Damn Yes, Vue allow us to pass those parameter as a component prop, but for this we have to config a thing.

     
    const router = createRouter({                             |
        history: createWebHistory(),                          V
        route: [
            { path: '/team/:teamId', component: Comp_Name, props: true }
        ]
    });



### Other properties can be found useful in route object: 

    1. Redirect:
    { path: '/', redirect: '/teams' }

    2. Alias
    { path: '/team/:teamId', component: Comp_Name, alias : '/' }   // All the properties aliaed(copied) to '/'

    3. REGEX
    { path: '/:notFound(.*)', redirect: '/team'  } 

    4. NAMED ROUTE
    { name: 'route-name', path: '/team', component: Hue_Hue }



#  Nested Routing: 
   Supppose a case to render a component along with another component and that will be through routing, then vue allows us to make this possible using nested routes. ( usually used for creating complex applications )

   const router = createRouter({

    history: createWebHistory(),
    routes: [

        { path: '/team', component: Comp_Name , children:[

 #           { path: ':tid', component: Child_Comp_Name }            `This path will be appended to the parent route`

        ] }

    ],

   });

   but again, till now we had discussed what and why and but not where, How does Vue know where to render this child component.
   for this we have to specify it by : <router-view></router-view>
   NOTE: this must be specified in the parent component.  this-case : Comp_Name




# The to=""  attribute of <router-link/>

    For simplicity it also accepts the js object so that we can configure our route in better way;

    ex: 
    <router-link to="myPath" ></router-link>

    computed: {

        myPath: function(){
            return { path:'', params: {  teamId:1 }  }

#           // or we can use named routes
            return { name: 'name-of-route' , params:{ teamId: 1 } } 

        }

    }


#   NAMED ROUTES
    For code reusability we simply can name any route we want by using `name` property. 




##  Query Params using Vue:

    We can simply use object method for simply add some query parameters to our URL, however we can do it simply also.

    myPath: function(){
        return { name: 'my-route', params: { tid: 'eng' },  query: { sort: 'asc' } };
    }

    and inside my-route's component we simply can utilise it using
#   this.$route.query.sort ( Opt. API )   ||   router.query.sort ( Comp. API )







###  What IF? we want to render multiple components on a same page using same route.

    Vue enable us to do so, go to the route configurations replace   `component` with 'components' and put the name of components you want to render in a object. like:

  0. Components: { router_link_name : Comp_Name,   router_link_name2: Comp_Name2,    default: Comp_Name3  }  

    // Yeah router-link also behaves same as slots in case of naming.
    In the parent Component file, use router-link as:-

   1. <router-link name="router_link_name" />
   2. <router-link name="router_link_name2" />
   3. <router-link />




## Control Scroll behaviour to enhance user experience, while navigating the tabs.

   While configuring router:

  const router = createRouter({

    history: createWebHistory(),
    route: [],
   `//Special Property`
    scrollBehaviour: function( to, from, savedPos )
    {
        console.log( to, savedPos, from );

        if( savedPos ){
            return savedPos;                                  // If you had already navigated to that page, just return there 
        }
        return { top: 0, left: 0 };                           // If the page is new look it from the top
    }  

  }) 





##  Navigation Guards:

    Can we restrict user to do navigation ? Yeas Vue provides us a power called Navigation guard to do so, but also remember Power comes with responsibility. So be careful while using it.

    What do we mean when i say Guard?  Simply it denotes all the functions that should be execute when any navigation action takes place.

    oke, so how do we do that?

# 1.    Again while configuring router, use a method `beforeEach`

      router.beforeEach( function( to, from , next ) {

        // to   : Contains the details of where we wanna go
        // from : Contains the details of location from where we are requesting to navigate
        // next : is a function that gives us power to restrict user.


        next()                                     -> allows the request 

        next('/team') || next( {path: '/team' } )  -> redirects user to '/team' 

        next(false)                                -> restrict user to navigate.
    
    }  );`


# 2.    Another method we have for guarding specific routes and for this we must configure that specific route,
      
      route: [

        { path: "", components: {}, beforeEnter: function( to, from, next ) {} }

      ]


# 3.   We also can add this same thing to the components life cycle

     export default {

        beforeRouteEnter: function( to, from, next ){

        }

     }

# 6  Here comes the phenomenal one:  This triggers when user tries to move into another URL

USE_CASE: Can use to ask a confirm request from the user whether he's sure to exit the site?   ||  You will lose your form data

      beforeRouteLeave: function( to, from, next )
      {
          next();
      }

# 4     In case of components if they are updated via routes, then beforeRouteUpdate() will triggered
      
       beforeRouteUpdate(): function( to, from , next ) { }


# 5   Likewise beforeEach we also have afterEach that executes once the we reach the route successfully:
  
      router.afterEach( function( to, from ) {} )     


    




