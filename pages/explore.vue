<template>
  <div id="app">
    <Header></Header>
    <main>
      <div class="container">
        <div class="row">
          <div class="col-12 text-center mt-5">
            <h2>Explore New Platforms</h2>
          </div>
          <div class="col-12 mb-3 mt-5">
            <el-select v-model="optionsValue" placeholder="Select">
              <el-option
                v-for="item in platformOptions"
                :key="item.value"
                :label="item.label"
                :value="item.value"
              >
              </el-option>
            </el-select>
          </div>

          <div
            class="col-md-3 mb-5"
            v-for="(value, index) in platforms"
            :key="index"
          >
            <div class="card" style="padding: 10px;">
              <a
                :href="value.website_url"
                target="_blank"
                style="padding: 10px; background: #edeff0;"
              >
                <img
                  :src="value.logo_url"
                  class="card-img-top"
                  :alt="value.name"
                />
              </a>
              <div class="card-body">
                <p class="card_text">
                  {{value.name}}
                </p>
                <p class="card-text">
                  {{ value.description }}
                </p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </main>
    <div class="bg-light mt-5">
      <Footer></Footer>
    </div>
  </div>
</template>

<script>
export default {
  name: "ExplorePage",
  head: {
    title: "Explore | Blockchain News",
  },
  components: {
    Headers: import("@/components/header.vue"),
    Footers: import("@/components/footer.vue"),
  },
  data() {
    return {
      platforms: [
        {
          name: "",
          description: "",
          logo_url: "",
          website_url: "",
          category: "",
          create_time: "",
          update_time: "",
        },
      ],
      platformOptions: [],
      optionsValue: "",
    };
  },
  created() {
    this.getPlatform();
    this.getBlockCategories();
  },
  methods: {
    getPlatform() {
      const url = "https://api.nowcoin.info/api/v1/platforms";

      this.$axios
        .get(url)
        .then((res) => {
          if (res && res.data && res.status === 200) {
            console.log(res.data);
            this.platforms = res.data;
          }
        })
        .catch((err) => {
          console.log(err);
        });
    },
    getBlockCategories() {
      const url = "https://api.nowcoin.info/api/v1/blockchain-categories";
      const self = this;
      self.$axios
        .get(url)
        .then((res) => {
          if (res.status === 200 && res.data) {
            res.data.map(function (value, key) {
              self.platformOptions.push({
                value: res.data[key].category,
                label: res.data[key].category,
              });
            });
          }
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
};
</script>