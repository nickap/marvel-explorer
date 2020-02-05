<template>
  <p>An infinite list of superb comics &#129322;</p>
</template>
<script>
/* eslint-disable */
export default {
  name: "MarvelExplorer",
  data() {
    return {
      url: "https://gateway.marvel.com/v1/public/comics"
    };
  },
  mounted: function() {
    this.fetchComics();
  },
  methods: {
    fetchComics: () => {
      fetch(
        "https://gateway.marvel.com/v1/public/comics?apikey=" +
          process.env.VUE_APP_API_KEY,
        {
          method: "GET"
        }
      )
        .then(data => {
          console.log("Success:", data);
        })
        .catch(error => {
          console.error("Error:", error);
        });
    }
  }
};
/* eslint-enable */
</script>