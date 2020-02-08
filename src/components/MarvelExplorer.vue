<template>
  <div class="container">
    <a name="top" class="anchor-link"></a>
    <p>An infinite list of superb comics &#129322;</p>
    <div v-show="sorted" class="row">
      <div class="d-flex flex-column col-12 col-sm-6 col-lg-3" v-for="(item, index) in data" :key="index">
        <div class="image-area">
          <img class="img" :src="item.image" :alt="'Image of ' + item.title"/>
          <p class="price" v-html="item.price"></p>
        </div>
        <div class="details-area mb-2">
          <p class="text-right m-0 small">Issue:&nbsp;{{item.issueNumber}}</p>
          <p class="text-left">{{item.title}}</p>
        </div>
      </div>
      <div ref="infiniteScrollTrigger"></div>
      <div class="loader" v-if="showLoader"></div>
    </div>
    <a href="#top" type="button" class="btn btn-secondary fixed-bottom mb-4 mr-5 btn-to-top">Top</a>
  </div>
</template>
<script>
/* Disable eslint enabling us to console.log() and to easier debug/review */
/* eslint-disable */
export default {
  name: "MarvelExplorer",
  data() {
    return {
      api: {
        url: "https://gateway.marvel.com/v1/public/comics",
        offset: 0
      },
      data: [],
      showLoader: false,
      showBtnTop: true
    };
  },
  mounted() {
    this.fetchComics().then(this.scrollTrigger());
  },
  computed: {
    sorted: function() {
      this.showLoader = false;
      return this.data.length > 0;
    }
  },
  methods: {
    fetchComics: function() {
      return new Promise((resolve, reject) => {
        fetch(
          this.api.url +
            "?offset=" +
            this.api.offset +
            "&apikey=" +
            process.env.VUE_APP_API_KEY,
          {
            method: "GET"
          }
        )
          .then(response => {
            this.api.offset += 20;
            return response.json();
          })
          .then(responseJson => {
            responseJson.data.results.forEach(item => {
              this.data.push({
                image: item.thumbnail.path + "." + item.thumbnail.extension,
                title: item.title,
                issueNumber: item.issueNumber,
                price:
                  item.prices[0].price == 0
                    ? "Sold Out"
                    : item.prices[0].price + "&nbsp;&euro;"
              });
            });
            console.log(this.data);
          })
          .catch(error => {
            console.error("Error:", error);
          });
      });
    },

    scrollTrigger() {
      const observer = new IntersectionObserver(entries => {
        entries.forEach(entry => {
          if (entry.intersectionRatio > 0) {
            this.showLoader = true;
            this.fetchComics();
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
.anchor-link {
  position: absolute;
  top: 0;
}
.btn-to-top {
  left: auto;
}
.image-area {
  position: relative;
  /* Aspect-ratio that all images will respect */
  padding-top: 155%;
  margin-bottom: 5px;
  box-shadow: 0 10px 20px -10px rgba(0, 0, 0, 0.7);
  overflow: hidden;
}
.image-area .img {
  position: absolute;
  display: block;
  height: 100%;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
}
.image-area .price {
  position: absolute;
  bottom: 10px;
  left: 10px;
  padding: 7px 10px;
  margin: 0px;
  background-color: #eee;
  box-shadow: 5px 5px 10px -5px rgba(0, 0, 0, 0.7);
}
.loader {
  margin: auto;
  margin-top: 50px;
  width: 50px;
  height: 50px;
  border-radius: 50%;
  border: 5px solid #6c757d;
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