<template>
  <section class="py-6 sm:py-12 text-gray-100">
    <div class="container p-6 mx-auto space-y-8">
      <div class="space-y-2 text-center">
        <h1 class="text-3xl font-bold text-gray-800">{{article.title}}</h1>
      </div>
      <div class="grid grid-cols-1 gap-x-4 gap-y-8 md:grid-cols-2 lg:grid-cols-4">
        <article
          class="flex flex-col bg-gray-900 rounded-xl"
          v-for="content in article.contents"
          :key="content.title"
        >
          <a :href="`/`+content.category+`/`+content.slug" aria-label="Te nulla oportere reprimique his dolorum">

            <nuxt-img
              class="object-cover w-full h-52 bg-gray-500 rounded-t-xl"
              :src="content.image"
              preload
              loading="lazy"
              width="850"
              height="620"
              format="webp"
              sizes="sm:100vw md:50vw lg:400px"
              :alt="content.title"
            />
          </a>
          <div class="flex flex-col flex-1 p-6">
            <a rel="noopener noreferrer" href="#" aria-label="Te nulla oportere reprimique his dolorum"></a>
            <a rel="noopener noreferrer" href="#" class="text-xs tracking-wider uppercase hover:underline text-violet-400">Convenire</a>
            <h3 class="flex-1 py-2 text-lg font-semibold leading-snug">
              <NuxtLink :to="{ name: content.category+'-slug', params: { slug: content.slug } }">
                {{ content.title }}
              </NuxtLink>
            </h3>
            <div class="flex flex-wrap justify-between pt-3 space-x-2 text-xs text-gray-400">
              <span>{{ content.created_at }}</span>
              <span>{{ content.read_duration }}</span>
            </div>
          </div>
        </article>
      </div>
      <p class=" text-sm text-gray-400">{{ article.description }}</p>
    </div>
  </section>
</template>
<script>

import siteMetaInfo from "@/data/sitemetainfo";
export default {
  data() {
    return {
      title: 0,
      siteMetadata: siteMetaInfo,
    };
  },
  async asyncData({ $content, params, $axios}) {
    const article = await $axios.$get(process.env.API_URL + '/accounts/tags/' + params.slug,{
      timeout: 5000
    })
    return {
      article: article.data,
    };
  },
  methods: {
    formatDate(date) {
      const options = { year: "numeric", month: "long", day: "numeric" };
      return new Date(date).toLocaleDateString("en", options);
    },
  },
  mounted() {

  },
  head() {
    //console.log(this.article.title);
    return {
      title: this.article.title,
      meta: [
        {
          hid: "description",
          name: "description",
          content: this.article.description,
        },
      ],
    };
  },
};
</script>
<style>
.nuxt-content h2 {
  font-weight: bold;
  font-size: 28px;
}
.nuxt-content h3 {
  font-weight: bold;
  font-size: 22px;
}
.nuxt-content p {
  margin-bottom: 20px;
}
</style>
