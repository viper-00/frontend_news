<template>
  <div id="app">
    <Header></Header>
    <main class="">
      <section class="py-5 text-center container">
        <div class="row py-lg-5">
          <div class="col-lg-6 col-md-8 mx-auto">
            <h1 class="fw-light">
              Learn something news about blockchain knowledges
            </h1>
            <p class="lead text-muted">
              The mainstream BTC, ETH, TRX, and Solana chains have tens of
              thousands of currency transactions every day. Understanding the
              latest market conditions and knowledge will help you make the most
              correct choice.
            </p>
            <p>
              <a href="/learn" class="btn btn-primary my-2">Learn more</a>
              <a href="/news" class="btn btn-secondary my-2 mx-2"
                >Get latest news</a
              >
            </p>
          </div>
        </div>
      </section>

      <div class="album py-5 bg-light">
        <div class="container">
          <div class="row row-cols-1 row-cols-sm-2 row-cols-md-3 g-3">
            <div class="col" v-for="(value, index) in articles" :key="index">
              <div class="card shadow-sm">
                <!-- <svg
                  class="bd-placeholder-img card-img-top"
                  width="100%"
                  height="225"
                  xmlns="http://www.w3.org/2000/svg"
                  role="img"
                  aria-label="Placeholder: Thumbnail"
                  preserveAspectRatio="xMidYMid slice"
                  focusable="false"
                >
                  <title>{{ value.title }}</title>
                  <rect width="100%" height="100%" fill="#55595c"></rect>
                  <text x="50%" y="50%" fill="#eceeef" dy=".3em">
                    Thumbnail
                  </text>
                </svg> -->
                <img :src="value.image_url" alt="card-img"/>
                <div class="card-body">
                  <p class="card-text">{{ value.description }}</p>
                  <div
                    class="d-flex justify-content-between align-items-center"
                  >
                    <div class="btn-group">
                      <button
                        type="button"
                        class="btn btn-sm btn-outline-secondary"
                      >
                        <NuxtLink :to="value.link"> See more </NuxtLink>
                      </button>
                    </div>
                    <small class="text-muted">{{ value.read_time }} 分钟</small>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </main>
    <Footer></Footer>
  </div>
</template>

<script>
export default {
  name: "IndexPage",
  head: {
    title: "Blockchain News",
  },
  components: {
    Headers: import("@/components/header.vue"),
    Footers: import("@/components/footer.vue"),
  },
  data() {
    return {
      articles: null,
    };
  },
  created() {
    this.getArticles();
  },
  methods: {
    getArticles() {
      const url = "https://api.nowcoin.info/api/v1/articles";
      this.$axios
        .get(url)
        .then((response) => {
          console.log(response);
          if (response && response.data && response.status === 200) {
            response.data.map(function (value, key) {
              response.data[key].link = "/article/" + response.data[key].id;
            });

            this.articles = response.data;
          }
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
};
</script>
