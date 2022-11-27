<template>
  <div id="app">
    <Header></Header>
    <main>
      <section class="container">
        <div class="row p-5">
          <div>
            <img
              :src="article.image_url"
              alt="background image"
              style="width: inherit"
            />
          </div>
          <h2 style="margin: 30px 0 20px">
            {{ article.title }}
            <span class="text-muted ml-2" style="font-size: 18px">
              {{ article.read_time }} minutes</span
            >
          </h2>
          <div v-html="markdownToHtml"></div>
        </div>
      </section>
    </main>
    <div class="bg-light mt-5">
      <Footer></Footer>
    </div>
  </div>
</template>

<script>
import { marked } from "marked";

export default {
  name: "articleIndex",
  head: {
    title: "Article Details | Blockchain News",
  },
  components: {
    Headers: import("@/components/header.vue"),
    Footers: import("@/components/footer.vue"),
  },
  data() {
    return {
      article: {
        content: "",
      },
    };
  },
  created() {
    const id = this.$route.params.id;
    if (id && parseInt(id) > 0) {
      this.getArticle(id);
    }
  },
  methods: {
    getArticle(id) {
      const url = "https://api.nowcoin.info/api/v1/article";
      this.$axios
        .get(url, {
          params: {
            article_id: id,
          },
        })
        .then((response) => {
          console.log(response);
          if (response && response.data && response.status === 200) {
            let article = response.data[0];
            article.content = decodeURIComponent(article.content);
            this.article = article;
          }
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
  computed: {
    markdownToHtml() {
      return marked(this.article.content);
    },
  },
};
</script>