<template>
  <div class="container">
    <p>An infinite list of superb comics &#129322;</p>
    <div v-show="sorted" class="row">
      <div class="col-6 col-md-3" v-for="(item, index) in data" :key="index">
        <img class="img-fluid" :src="item.image"/>
      </div>
    </div>
  </div>
</template>
<script>
/* eslint-disable */
export default {
  name: "MarvelExplorer",
  data() {
    return {
      url: "https://gateway.marvel.com/v1/public/comics?apikey=",
      data: []
    };
  },
  created: function() {
    this.fetchComics();
  },
  computed: {
    sorted: function() {
      return this.data.length > 0;
    }
  },
  methods: {
    fetchComics: function() {
      fetch(this.url + process.env.VUE_APP_API_KEY, {
        method: "GET"
      })
        .then(response => {
          return response.json();
        })
        .then(responseJson => {
          responseJson.data.results.forEach(item => {
            this.data.push({
              image: item.thumbnail.path + "." + item.thumbnail.extension,
              title: item.title,
              issueNumber: item.issueNumber,
              price: item.prices.price
            });
          });
          console.log(this.data);
        })
        .catch(error => {
          console.error("Error:", error);
        });
    }
  }
};
/* eslint-enable */
</script>