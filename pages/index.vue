<template>
  <news-preview
    v-for="item of news"
    :key="item.id"
    :news="item"
    @news-change="getNewsList()"
  />

  <news-add v-if="user" @news-change="getNewsList()" />
</template>

<script lang="ts">
import { NewsInterface } from "interfaces/News.interface";

export default {
  async setup() {
    useHead({
      title: "Wiadomości",
    });

    const { user, setIsLoading } = useAppState();
    const { getNewsListApi } = useNewsApi();

    const news = ref<NewsInterface[]>([]);

    const getNewsList = async (): Promise<void> => {
      try {
        setIsLoading(true);
        const { data } = await useAsyncData<NewsInterface[]>(() =>
          getNewsListApi()
        );

        if (data.value) {
          news.value = [...data.value];
        }
      } catch (err: any) {
        console.error(`ERROR:${err}`);
      } finally {
        setIsLoading(false);
      }
    };
    await getNewsList();

    return {
      user,
      news,
      getNewsList,
    };
  },
};
</script>
