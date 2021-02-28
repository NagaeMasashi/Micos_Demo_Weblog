<template>
  <section class="latest-posts">
    <div class="posts">
      <nuxt-link :to="`/${content.id}`" class="post" v-for="content in contents" :key="content.id">
        <div class="thumb">
          <img :src="content.image.url">
        </div>
        <div class="post-text">
          <p>{{ formatDate(content.date) }}</p>
          <h2>{{ content.title }}</h2>
        </div>
      </nuxt-link>
    </div>
  </section>
</template>

<script>
import axios from 'axios'
export default {
  async asyncData({ params }) {
    const page = params.p || '1'
    const limit = 10
    const { data } = await axios.get(
      `https://balsamicos.microcms.io/api/v1/weblog?limit=${limit}&offset=${(page - 1) * limit}`,
      { headers: { 'X-API-KEY': '0e9f60c7-d465-4e0a-b99c-9b89ffb98501' } }
    )
    return data
  },
  methods: {
    formatDate(iso) {
      const date = new Date(iso)
      const yyyy = new String(date.getFullYear())
      const mm = new String(date.getMonth() + 1).padStart(2, "0")
      const dd = new String(date.getDate()).padStart(2, "0")
      return `${yyyy}.${mm}.${dd}`
    }
  }
}
</script>

<style lang="scss">
section.latest-posts {
  padding: 10px;
  .posts {
    max-width: 900px;
    margin: 0 auto;
    padding: 10px;
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    background: #ddd;
    a.post {
      width: calc(100% / 2 - 20px);
      @media (min-width: (768px)) {
        width: calc(100% / 3 - 20px);
      }
      margin: 10px;
      background: #fff;
      text-decoration: none;
      color: #111;
      .thumb {
        width: 100%;
        padding-bottom: 75%;
        position: relative;
        overflow: hidden;
        img {
          position: absolute;
          top: 50%;
          left: 50%;
          transform: translate(-50%, -50%);
          max-width: 100%;
        }
      }
      .post-text {
        padding: 5px 10px 10px;
        h2 {
          width: fit-content;
          font-size: 20px;
        }
      }
    }
  }
}
</style>
