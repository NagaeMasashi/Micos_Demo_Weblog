<template>
  <main class="main">
    <h1 class="title">{{ title }}</h1>
    <p class="publishedAt">{{ formatDate(date) }}</p>
    <div class="post" v-html="body"></div>
  </main>
</template>

<script>
import axios from "axios";

export default {
  async asyncData({ params }) {
    const { data } = await axios.get(
      `https://balsamicos.microcms.io/api/v1/weblog/${params.slug}`,
      {
        headers: { "X-API-KEY": "0e9f60c7-d465-4e0a-b99c-9b89ffb98501" }
      }
    );
    return data;
  },
  methods: {
    formatDate(iso) {
      const date = new Date(iso);
      const yyyy = new String(date.getFullYear());
      const mm = new String(date.getMonth() + 1).padStart(2, "0");
      const dd = new String(date.getDate()).padStart(2, "0");
      return `${yyyy}.${mm}.${dd}`;
    }
  }
};
</script>

<style lang="scss" scoped>
.main {
    max-width: 900px;
    margin: 0 auto;
    padding: 10px;
    margin: 0 auto;
    .title {
    margin-bottom: 20px;
  }

  .publishedAt {
    margin-bottom: 40px;
  }

  .post {
    h1 {
      font-size: 30px;
      font-weight: bold;
      margin: 40px 0 20px;
      background-color: #eee;
      padding: 10px 20px;
      border-radius: 5px;
    }

    h2 {
      font-size: 24px;
      font-weight: bold;
      margin: 40px 0 16px;
      border-bottom: 1px solid #ddd;
    }

    p {
      line-height: 1.8;
      letter-spacing: 0.2px;
    }

    ol {
      list-style-type: decimal;
      list-style-position: inside;
    }

    
    img {
      max-width: 100%;
    }
  }
}
</style>
