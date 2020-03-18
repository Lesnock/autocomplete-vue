# autocomplete-vue
An autocomplete input made with Vue.js

Based on Afik Deri's tutorial:  
https://github.com/AfikDeri/VueJS-Autocomplete

It has some differences in the style and html elements compared to Afik Deri's auto complete,  
But the logic is almost the same.

## Usage
```html
<Autocomplete
  filterBy="name"
  :items="[
      { name: 'Blue' },
      { name: 'Red' },
      { name: 'Yellow' },
      { name: 'Purple' },
  ]"
/>
```

Basically, it is required to pass two props to the Autocomplete component:  
* items - An array of objects, that will be used to search for when typing in the Autocomplete input.
* filterBy - The key of the objects that Autocomplete will use for search for.
