<template>
  <div id="app">
    <el-container>
      <el-header style="padding: 20px">
        <el-page-header @back="goBack" content="发布页面"> </el-page-header>
      </el-header>
      <el-main>
        <el-form
          :label-position="labelPosition"
          :model="articleForm"
          :rules="articleRules"
          ref="articleForm"
          label-width="100px"
          class="demo-platformForm"
        >
          <el-form-item label="标题" prop="title">
            <el-input v-model="articleForm.title"></el-input>
          </el-form-item>
          <el-form-item label="简单描述" prop="description">
            <el-input v-model="articleForm.description"></el-input>
          </el-form-item>
          <el-form-item label="图片地址" prop="imageUrl">
            <el-input v-model="articleForm.imageUrl"></el-input>
          </el-form-item>
          <el-row :gutter="20">
            <el-col :span="12">
              <el-form-item label="内容" prop="content">
                <el-input
                  type="textarea"
                  v-model="articleForm.content"
                  :rows="5"
                ></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="12">
              <div
                style="grid-content bg-purple-light"
                v-html="markdownToHtml"
              ></div>
            </el-col>
          </el-row>

          <el-form-item label="阅读时间" prop="readTime">
            <el-input-number
              v-model="articleForm.readTime"
              @change="handleChange"
              :min="1"
              :max="20"
              label="阅读时间"
            ></el-input-number>
          </el-form-item>

          <el-form-item>
            <el-button type="primary" @click="submitArticleForm('articleForm')"
              >立即发布</el-button
            >
            <el-button @click="resetForm('articleForm')">重置</el-button>
          </el-form-item>
        </el-form>
      </el-main>

      <el-header style="padding: 20px">
        <el-page-header @back="goBack" content="发布平台"> </el-page-header>
      </el-header>
      <el-main>
        <el-form
          :label-position="labelPosition"
          :model="platformForm"
          label-width="100px"
          class="demo-platformForm"
          ref="platformForm"
          :rules="platformRules"
        >
          <el-form-item label="名称" prop="name">
            <el-input v-model="platformForm.name"></el-input>
          </el-form-item>
          <el-form-item label="描述" prop="description">
            <el-input v-model="platformForm.description"></el-input>
          </el-form-item>
          <el-form-item label="Logo 地址" prop="logoUrl">
            <el-input v-model="platformForm.logoUrl"></el-input>
          </el-form-item>
          <el-form-item label="网站地址" prop="websiteUrl">
            <el-input v-model="platformForm.websiteUrl"></el-input>
          </el-form-item>
          <el-form-item label="类别" prop="category">
            <el-select v-model="platformForm.category" placeholder="请选择">
              <el-option
                v-for="item in platformCategoryOptions"
                :key="item.value"
                :label="item.label"
                :value="item.value"
              >
              </el-option>
            </el-select>
          </el-form-item>
          <el-form-item>
            <el-button
              type="primary"
              @click="submitPlatformForm('platformForm')"
              >立即发布</el-button
            >
            <el-button @click="resetForm('platformForm')">重置</el-button>
          </el-form-item>
        </el-form>
      </el-main>
    </el-container>
  </div>
</template>

<script>
import { marked } from "marked";
import * as DOMPurify from "dompurify";

export default {
  name: "adminPage",
  head: {
    title: "Admin | Blockchain News",
  },
  data() {
    return {
      labelPosition: "left",
      articleForm: {
        title: "",
        content: "",
        imageUrl: "",
        description: "",
        readTime: 1,
      },
      platformForm: {
        title: "",
        description: "",
        logoUrl: "",
        websiteUrl: "",
        category: "",
      },
      articleRules: {
        title: [{ required: true, message: "请填写标题", trigger: "change" }],
        content: [{ required: true, message: "请填写内容", trigger: "change" }],
        description: [
          { required: true, message: "请填写描述", trigger: "change" },
        ],
        readTime: [
          { required: true, message: "请选择阅读时间", trigger: "change" },
        ],
        imageUrl: [
          { required: true, message: "请填写图片地址", trigger: "change" },
        ],
      },
      platformRules: {
        name: [
          { required: true, message: "请填写网站名称", trigger: "change" },
        ],
        description: [
          { required: true, message: "请填写描述", trigger: "change" },
        ],
        logoUrl: [
          { required: true, message: "请填写 Logo 地址", trigger: "change" },
        ],
        websiteUrl: [
          { required: true, message: "请填写网站地址", trigger: "change" },
        ],
        category: [
          { required: true, message: "请选择类别", trigger: "change" },
        ],
      },
      platformCategoryOptions: [],
    };
  },
  created() {
    this.getBlockCategories();
  },
  methods: {
    goBack() {
      console.log("go back");
    },
    submitArticleForm(formName) {
      const self = this;
      this.$refs[formName].validate((valid) => {
        if (valid) {
          const url = "http://127.0.0.1:8888/api/v1/article";
          this.$axios
            .post(
              url,
              {
                title: self.articleForm.title,
                content: encodeURIComponent(
                  DOMPurify.sanitize(marked.parse(self.articleForm.content))
                ),
                description: self.articleForm.description,
                readTime: self.articleForm.readTime,
                imageUrl: self.articleForm.imageUrl,
              },
              {
                headers: {
                  "Content-Type": "application/json; charset=utf-8",
                },
              }
            )
            .then((response) => {
              if (
                response.status === 200 &&
                response.data &&
                parseInt(response.data.insertId) > 0
              ) {
                alert("Insert successful!");
                self.$refs[formName].resetFields();
                return;
              }
              alert("Error: " + JSON.stringify(response.data));
            })
            .catch((err) => {
              console.log(err);
            });
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },
    submitPlatformForm(formName) {
      const self = this;
      this.$refs[formName].validate((valid) => {
        if (valid) {
          const url = "http://127.0.0.1:8888/api/v1/platform";
          this.$axios
            .post(
              url,
              {
                name: self.platformForm.name,
                description: self.platformForm.description,
                logoUrl: self.platformForm.logoUrl,
                websiteUrl: self.platformForm.websiteUrl,
                category: self.platformForm.category,
              },
              {
                headers: {
                  "Content-Type": "application/json; charset=utf-8",
                },
              }
            )
            .then((res) => {
              if (
                res.status === 200 &&
                res.data &&
                parseInt(res.data.insertId) > 0
              ) {
                alert("publish successful!");
                self.$refs[formName].resetFields();
                return;
              }
            })
            .catch((err) => {
              console.log(err);
            });
        }
      });
    },
    resetForm(formName) {
      this.$refs[formName].resetFields();
    },
    handleChange(value) {
      console.log(value);
    },
    getBlockCategories() {
      const url = "http://127.0.0.1:8888/api/v1/blockchain-categories";
      const self = this;
      self.$axios
        .get(url)
        .then((res) => {
          if (res.status === 200 && res.data) {
            res.data.map(function (value, key) {
              self.platformCategoryOptions.push({
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
  computed: {
    markdownToHtml() {
      return marked(this.articleForm.content);
    },
  },
};
</script>
