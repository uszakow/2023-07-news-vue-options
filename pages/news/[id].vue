<template>
  <app-error v-if="!news" />
  <template v-else>
    <ui-typography type="title">
      {{ news?.title }}
    </ui-typography>

    <p v-for="(item, index) of content" :key="index + item" class="news-p">
      {{ item }}
    </p>

    <div className="mt-2">
      Autor: <span class="author">{{ news?.author.name }}</span>
    </div>
  </template>
</template>

<script lang="ts">
import { NewsInterface } from "interfaces/News.interface";

export default {
  computed: {
    content(): string[] {
      if (this.news) {
        return this.news?.content.split("\n");
      }
      return [];
    },
  },
  async setup() {
    const { params } = useRoute();
    const { setIsLoading } = useAppState();
    const { getNewsApi } = useNewsApi();

    const newsId = params?.id as string;
    const news = ref<NewsInterface>();

    try {
      setIsLoading(true);
      const { data } = await useAsyncData<NewsInterface>(() =>
        getNewsApi(newsId)
      );

      if (data.value) {
        news.value = data.value;
      }
    } catch (err: any) {
      console.error(`ERROR:${err}`);
    } finally {
      setIsLoading(false);

      useHead({
        title: news.value?.title || "News app",
      });
    }

    return {
      news,
    };
  },
};
</script>

<style lang="scss" scoped>
.news-p {
  text-indent: 2rem;
}
.author {
  font-style: italic;
}
</style>
