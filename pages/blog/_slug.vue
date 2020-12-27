<template>
  <article>
    <h1>{{ article.title }}</h1>
    <p>{{ article.date }}</p>
    <nuxt-content :document="article" />
  </article>
</template>

<script lang="ts">
export default {
  async asyncData({ $content, params }) {
    const article = await $content('blog', params.slug).fetch()

    return { article }
  },
}
</script>
