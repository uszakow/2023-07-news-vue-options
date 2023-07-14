<template>
  <ui-button
    classes="mt-2"
    type="adding"
    label="Dodaj nową wiadomość"
    @click-button="isModalOpen = true"
  />

  <news-manage-modal
    :is-open="isModalOpen"
    modal-title="Dodanie nowej wiadomości"
    :news-title="newsTitle"
    :news-content="newsContent"
    :loading="loading"
    :error="error"
    @change-news-title="newsTitle = $event"
    @change-news-content="newsContent = $event"
    @manage-news="createNews()"
    @close-modal="closeModal()"
  />
</template>

<script lang="ts">
import { NewsDto } from "interfaces/News.dto";

export default {
  emits: ["newsChange"],
  data() {
    return {
      isModalOpen: false,
      newsTitle: "",
      newsContent: "",
      loading: false,
      error: "" as string | string[],
    };
  },
  methods: {
    async createNews(): Promise<void> {
      const body: NewsDto = {
        title: this.newsTitle,
        content: this.newsContent,
      };

      try {
        this.loading = true;

        if (this.token) {
          await this.createNewsApi(body, this.token);
        }

        this.$emit("newsChange");

        this.newsTitle = "";
        this.newsContent = "";
        this.isModalOpen = false;
      } catch (err: any) {
        this.error = err.data?.message || "Nie udało się stworzyć wiadomość.";
      } finally {
        this.loading = false;
      }
    },
    closeModal(): void {
      this.error = "";
      this.isModalOpen = false;
    },
  },
  setup() {
    const { token } = useAppState();
    const { createNewsApi } = useNewsApi();

    return {
      token,
      createNewsApi,
    };
  },
};
</script>
