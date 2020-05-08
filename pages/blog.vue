<template>
  <div >
   <p>
       Area is equal to: {{ area }}
     </p>
     <p>
       <button v-on:click="length += 1">Add length</button>
       <button v-on:click="width += 1">Add width</button>
     </p>
     <!--
         We can see the lifecycle in action by using watch functions to observe when the data in an instance changes, and the beforeUpdate lifecycle hook, which should be executed only after the data has changed.

        INFO A watch function works like a lifecycle hook, but it’s triggered when the data it’s “watching” is updated. We can even create a watch function to observe a computed property.

        Listing 2 puts our area calculation in the context of a complete application. The application also contains three watch functions that log messages to the console whenever length, width, or area change, and one function to log when the update cycle begins. These functions must be specified in the watch option of the Vue instance for them to work.

        TIP You can find the code for this listing in the samples that accompany this article in the file chapter-03/area.html. It’s entirely self-contained, and you can open it directly in Chrome.

        https://freecontent.manning.com/examining-update-events-with-computed-properties-in-vue-js/
    --->
   </div>
</template>
<script>
export default {
    name: "blog",
    components: {},
    data: function(){
    	return {
      	    length: 5,
            width: 3
        };
    },
    computed: {
        area: function() {
            console.log("claculate");
            return this.width * this.length;

        }
    },
    watch: {
        length: function(newVal, oldVal) {
           console.log('The old value of length was: '
                       + oldVal +
                       '\nThe new value of length is: '
                       + newVal);
        },
        width: function(newVal, oldVal) {
           console.log('The old value of width was: '
                       + oldVal +
                       '\nThe new value of width is: '
                       + newVal);
        },
        area: function(newVal, oldVal) {
           console.log('The old value of area was: '
                       + oldVal +
                       '\nThe new value of area is: '
                       + newVal);
        },

    },
    beforeUpdate: function() {
       console.log('All those data changes happened '
                     + 'before the output gets updated.');
    }
};
</script>
<style scoped>
</style>
