<template>
  <div class="container">
    <p>An infinite list of superb comics &#129322;</p>
    <div v-show="sorted" class="row">
      <div class="col-6 col-md-3" v-for="(item, index) in data" :key="index">
        <img class="img-fluid" :src="item.image" />
      </div>
      <div ref="infiniteScrollTrigger" id="scroll-trigger"></div>
      <div class="loader" v-if="showLoader"></div>
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
      data: [],
      // currentPage: 1,
      // totalPages: 4,
      showLoader: false
    };
  },
  created: function() {
    this.fetchComics();
  },
  mounted() {
    this.scrollTrigger();
    this.showLoader = false;
  },
  computed: {
    sorted: function() {
      return this.data.length > 0;
    },
    // pageCount() {
    //   return this.totalPages - this.currentPage;
    // }
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
    },

    scrollTrigger() {
      const observer = new IntersectionObserver(entries => {
        entries.forEach(entry => {
          if (entry.intersectionRatio > 0) {
            this.showLoader = true;
            this.fetchComics();
            this.currentPage += 1;
          }
        });
      });
      observer.observe(this.$refs.infiniteScrollTrigger);
    }
  }
};
/* eslint-enable */
</script>
<style lang="css" scoped>
.loader {
  margin: auto;
  margin-top: 50px;
  width: 50px;
  height: 50px;
  border-radius: 50%;
  border: 5px solid rgba(125, 125, 125, 0.7);
  border-top: 5px solid #eee;
  animation: animate 1s infinite linear;
}
@keyframes animate {
  0% {
    transform: translate(-50%, -50%) rotate(0deg);
  }
  100% {
    transform: translate(-50%, -50%) rotate(360deg);
  }
}
</style>