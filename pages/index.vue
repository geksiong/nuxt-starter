<template>
  <div class="prose">
    <h1>Blog Posts</h1>
    <ul>
      <li v-for="article of articles" :key="article.slug">
        <NuxtLink :to="{ name: 'blog-slug', params: { slug: article.slug } }">
          <div>
            <h2>{{ article.title }}</h2>
            <p>{{ article.date }}</p>
            <p>{{ article.description }}</p>
          </div>
        </NuxtLink>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  async asyncData({ $content, params }) {
    const articles = await $content('blog', params.slug)
      .only(['title', 'description', 'date', 'slug'])
      .sortBy('date', 'desc')
      .fetch()

    return {
      articles,
    }
  },
}
</script>
