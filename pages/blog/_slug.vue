<template>
  <article class="prose lg:prose-xl">
    <h1>{{ article.title }}</h1>
    <p>{{ article.date }}</p>
    <nuxt-content :document="article" />
  </article>
</template>

<script>
export default {
  async asyncData({ $content, params }) {
    const article = await $content('blog', params.slug).fetch()

    return { article }
  },
}
</script>
