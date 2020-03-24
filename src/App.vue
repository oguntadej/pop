<template>
  <div id="app">
   <div class="container">
     <div class="row my-5 justify-content-center">
       <h1 class="col-md-12 display-4 py-4 font-weight-bold">Hello, I'm Joshua,
         <small>I'me, I do me and I chill.</small></h1>
       <div class="col-md-6 py-5 about">
         <p>
           These days, I am working on engineering,
           product development and business strategy at
           Pennysmart, a digital saving platform for African millennials.
         </p>
         <p>
           I can generally be found listening to an unhealthy amount of music and reading on various subjects.
         </p>
         <p><span v-if="book.records">I'm reading
           <i class="font-weight-bold">{{currentlyReading}}</i> at the moment.</span>
           Feel free to check out <a href="https://airtable.com/shrLvsksJPIW2H9ST" target="_blank">my library.</a> </p>
       </div>
     </div>
   </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'app',
  components: {},
  data() {
    return {
      book: [],
    };
  },
  methods: {
    fetchCurrentlyReading() {
      const uri = 'https://api.airtable.com/v0/appE7rDWQTMztAodU/Books';
      const token = process.env.VUE_APP_AIRTABLE_TOKEN;
      axios.get(uri, { headers: { Authorization: `Bearer ${token}` }, params: { fields: ['Title'], filterByFormula: 'AND({Status} = "Reading")' } })
        .then((response) => { this.book = response.data; }).catch(error => (console.log(error)));
    },
  },
  computed: {
    currentlyReading() {
      let currentlyReading = 'nothing';
      if (this.book.records.length > 0) {
        currentlyReading = this.book.records[0].fields.Title;
      }
      return currentlyReading;
    },
  },
  mounted() {
    this.fetchCurrentlyReading();
  },
};
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #dddddd;
}
  .background-dark{
    background-color: rgb(10,10,10);
  }
  .portrait{
    width: 300px;
    height: auto;
  }
  .about{
    text-align: justify;
  }
</style>
