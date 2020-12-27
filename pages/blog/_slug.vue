<template>
  <article class="prose lg:prose-xl">
    <h1>{{ article.title }}</h1>
    <p>Updated {{ formatDate(article.date) }}</p>

    <!-- table of contents -->
    <nav>
      <ul>
        <li v-for="link of article.toc" :key="link.id">
          <NuxtLink
            :to="`#${link.id}`"
            :class="{ 'py-2': link.depth === 2, 'ml-4': link.depth === 3 }"
            >{{ link.text }}</NuxtLink
          >
        </li>
      </ul>
    </nav>

    <nuxt-content :document="article" />

    <!-- previous/next links -->
    <div class="flex justify-between">
      <NuxtLink
        v-if="prev"
        :to="{ name: 'blog-slug', params: { slug: prev.slug } }"
        class="font-bold text-primary hover:underline"
      >
        {{ prev.title }}
      </NuxtLink>
      <span v-else>&nbsp;</span>
      <NuxtLink
        v-if="next"
        :to="{ name: 'blog-slug', params: { slug: next.slug } }"
        class="font-bold hover:underline"
      >
        {{ next.title }}
      </NuxtLink>
      <span v-else>&nbsp;</span>
    </div>
  </article>
</template>

<script>
export default {
  async asyncData({ $content, params }) {
    const article = await $content('blog', params.slug).fetch()

    const [prev, next] = await $content('blog')
      .only(['title', 'slug'])
      .sortBy('createdAt', 'asc')
      .surround(params.slug)
      .fetch()

    return { article, prev, next }
  },
  methods: {
    formatDate(date) {
      const options = { year: 'numeric', month: 'short', day: 'numeric' }
      return new Date(date).toLocaleDateString('en', options)
    },
  },
}
</script>

<style>
.math-inline svg {
  display: inline;
}
</style>
