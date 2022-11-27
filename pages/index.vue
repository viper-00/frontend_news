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

      <section class="pt-5 pb-5">
        <div class="container">
          <div class="row row-cols-1 row-cols-sm-2 row-cols-md-4">
            <div
              class="col-md-3 mt-2"
              v-for="(value, index) in prices"
              :key="index"
            >
              <div class="card shadow-sm card-body">
                <p>
                  <img :src="value.image" alt="prices" width="40px" height="40px"/>
                  <span style="float: right">
                    <small style="margin-right: 20px">Buy</small>
                    <small>Trade</small>
                  </span>
                </p>
                <p class="card-text">
                  {{ value.name }}
                  <span class="text-muted ml-2">{{ value.id }}</span>
                </p>
                <p class="card-text">{{ value.basePair }} {{ value.value }}</p>
              </div>
            </div>
          </div>
        </div>
      </section>

      <section class="album py-5 bg-light">
        <div class="container">
          <h2>Latest News</h2>
          <div class="row row-cols-1 row-cols-sm-2 row-cols-md-3 g-3 mt-3">
            <div class="col" v-for="(value, index) in articles" :key="index">
              <div class="card shadow-sm">
                <img :src="value.image_url" alt="card-img" />
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
      </section>
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
      prices: [],
      timer: '',
    };
  },
  created() {
    this.getArticles();
    this.getCoinExchangeRate();
  },
  methods: {
    getArticles() {
      const url = "https://api.nowcoin.info/api/v1/articles";
      this.$axios
        .get(url)
        .then((response) => {
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
    getCoinExchangeRate() {
      const basePair = "USD";
      this.prices = [];
      const base_url = "https://api.nowcoin.info/api/v1/coin-exchange-rate?";
      // BTC
      this.$axios
        .get(base_url + "currency=BTC&basePair=" + basePair)
        .then((btc_res) => {
          if (btc_res && btc_res.data && btc_res.status === 200) {
            btc_res.data.name = "Bitcoin";
            btc_res.data.image = "/img/prices/prices-btc.svg";
            this.prices.push(btc_res.data);

            // ETH
            this.$axios
              .get(base_url + "currency=ETH&basePair=" + basePair)
              .then((eth_res) => {
                if (eth_res && eth_res.data && eth_res.status === 200) {
                  eth_res.data.name = "Ethereum";
                  eth_res.data.image = "/img/prices/prices-eth.svg";
                  this.prices.push(eth_res.data);

                  // XLM
                  this.$axios
                    .get(base_url + "currency=XLM&basePair=" + basePair)
                    .then((xml_res) => {
                      if (xml_res && xml_res.data && xml_res.status === 200) {
                        xml_res.data.name = "Steller";
                        xml_res.data.image = "/img/prices/prices-xlm.svg";
                        this.prices.push(xml_res.data);

                        // SOL
                        this.$axios
                          .get(base_url + "currency=SOL&basePair=" + basePair)
                          .then((sol_res) => {
                            if (
                              sol_res &&
                              sol_res.data &&
                              sol_res.status === 200
                            ) {
                              sol_res.data.name = "Solana";
                              sol_res.data.image = "/img/prices/prices-sol.svg";
                              this.prices.push(sol_res.data);
                            }
                          })
                          .catch((sol_err) => {
                            console.log(sol_err);
                          });
                      }
                    })
                    .catch((xlm_err) => {
                      console.log(xlm_err);
                    });
                }
              })
              .catch((eth_err) => {
                console.log(eth_err);
              });
          }
        })
        .catch((btc_err) => {
          console.log(btc_err);
        });
    },
  },
  mounted(){
    this.timer = setInterval(this.getCoinExchangeRate, 30000);
  },
  beforeDestroy() {
    clearInterval(this.timer)
  }
};
</script>
