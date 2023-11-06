Rendering Conditional Content to the UI.
In Web development, it is a hectic and crucial task to render our data conditionally.
Let's make it easy with Vue.js

here, we have multiple directives  as follows :->

<html_element  v-if="condition"  />
<html_element  v-else-if="condition"  />
<html_element  v-else />

It will render those statement which returns out to be true.

______________________________________________________________________________________________________________________

->There is also has an Conditional render called ` v-show="_condition_" `;
-> Its output is same as like v-if is having, so what's the difference? 
    v-if really removes the element from the DOM while v-show just changes the style property 'display': none;
-> So ideally we must use v-if for conditional renders and only use v-show for that element whose style property changes regulary.



______________________________________________________________________________________________________________________

There is another directive called v-for: Used to iterate any iterables( array, object etc )

ex: <ul>
        <li v-for=" item in arr "> {{ item }} </li> 
    </ul>
-> It will list all the items that residing inside arr array as a list element.

=> The coolest thing is that Vue does it very efficiently, it only adds the element to the DOM without touching any unchanged element. This makes Vue Very Performant

Iterables - Arrays, objects, Numbers

Arrays:
 <li v-for="(todo, idx) in todos" >{{ todo }} - {{ idx }} </li>

Objects:
<li v-for=" ( value, key, idx ) in { name:'shivang', age: '20'} ">{{ key }}: {{ value }} {{ idx }}</li>

Numbers:
<li v-for=" item in 10 " > {{ item }} </li>




_________________________________________________________________________________________________________________________

BUG TIP : ` while using v-for `

Suppose the item which is iterating contains its child elements.
ex: <li v-for=" (item,idx) in arr" >
        {{ item }}
        <br/>
        <input /> 
    </li>

Now Consider a scenerio where we have two elements and I removed the first element then what happens is, the dynamic content of first element is successfully changed but
the input data remains exist there.

Actually what happening is Vue is just moving the dynamic contents of second to the first and delete the last one to improve the performance. that's why The input data remains same.
To resolve this BIG_BUG use key attribute. It maps the uniques value to each element of List and now it can easily solve this issue.

<li v-for=" (item,idx) in arr" v-bind:key=" item " > {{ item }} </li>
idx can be same for two elements that's why we didn't pass idx in the key attribute value

