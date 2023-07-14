<template>
  <div class="news-preview p-1 mb-2">
    <ui-typography
      classes="flex flex-justify-between flex-align-end"
      type="title"
    >
      <nuxt-link :to="`/news/${news.id}`" class="news-preview-title">
        {{ news.title }}
      </nuxt-link>

      <div v-if="user?.id === news.author.id" class="flex">
        <ui-button
          type="inline"
          label="Edytuj"
          @click-button="isEditNewsModalOpen = true"
        />
        <ui-button
          classes="ml-2"
          type="inline"
          label="Usuń"
          @click-button="isDeleteNewsModalOpen = true"
        />
      </div>
    </ui-typography>

    <div class="news-preview-content">
      <p v-for="(item, index) of content" :key="index + item">
        {{ item }}
      </p>
    </div>

    <news-manage-modal
      :is-open="isEditNewsModalOpen"
      modal-title="Edytowanie wiadomości"
      :news-title="updatedNewsTitle"
      :news-content="updatedNewsContent"
      :loading="loading"
      :error="error"
      @change-news-title="updatedNewsTitle = $event"
      @change-news-content="updatedNewsContent = $event"
      @manage-news="updateNews()"
      @close-modal="closeEditNewsModal()"
    />

    <ui-modal
      title="Usuń wiadomość"
      :is-open="isDeleteNewsModalOpen"
      @close-modal="closeDeleteNewsModal()"
    >
      <ui-typography type="text">
        Potwierdź usunięcie tej wiadomości.
      </ui-typography>

      <ui-button
        classes="mt-2 m-auto"
        type="danger"
        label="Usuń wiadomość"
        :loading="loading"
        @click-button="deleteNews()"
      />

      <ui-error-message v-if="error" :message="error" />
    </ui-modal>
  </div>
</template>

<script lang="ts">
import { NewsInterface } from "interfaces/News.interface";
import { UpdateNewsDto } from "interfaces/UpdateNews.dto";

export default {
  props: {
    news: {
      type: Object as () => NewsInterface,
      required: true,
    },
  },
  emits: ["newsChange"],
  data() {
    return {
      isEditNewsModalOpen: false,
      isDeleteNewsModalOpen: false,
      updatedNewsTitle: this.$props.news.title,
      updatedNewsContent: this.$props.news.content,
      loading: false,
      error: "" as string | string[],
    };
  },
  computed: {
    content(): string[] {
      return this.$props.news.content.split("\n");
    },
  },
  methods: {
    async updateNews(): Promise<void> {
      const body: UpdateNewsDto = {
        title: this.updatedNewsTitle,
        content: this.updatedNewsContent,
      };

      try {
        this.loading = true;

        if (this.token) {
          await this.updateNewsApi(this.$props.news.id, body, this.token);
        }

        this.$emit("newsChange");

        this.isEditNewsModalOpen = false;
      } catch (err: any) {
        this.error =
          err.data?.message || "Nie udało się zaktualizować wiadomość.";
      } finally {
        this.loading = false;
      }
    },
    async deleteNews(): Promise<void> {
      try {
        this.loading = true;

        if (this.token) {
          await this.deleteNewsApi(this.$props.news.id, this.token);
        }

        this.$emit("newsChange");

        this.isDeleteNewsModalOpen = false;
      } catch (err: any) {
        this.error = err.data?.message || "Nie udałos się usunąć wiadomość.";
      } finally {
        this.loading = false;
      }
    },
    closeEditNewsModal(): void {
      this.error = "";
      this.updatedNewsTitle = this.$props.news.title;
      this.updatedNewsContent = this.$props.news.content;
      this.isEditNewsModalOpen = false;
    },
    closeDeleteNewsModal(): void {
      this.error = "";
      this.isDeleteNewsModalOpen = false;
    },
  },
  setup() {
    const { user, token } = useAppState();
    const { updateNewsApi, deleteNewsApi } = useNewsApi();

    return {
      user,
      token,
      updateNewsApi,
      deleteNewsApi,
    };
  },
};
</script>

<style lang="scss" scoped>
@import "styles/variables";
@import "styles/mixins";

.news-preview {
  border: 2px solid $main;
  border-radius: $border-radius;

  &-title {
    @include text-one-line;
    color: $main;

    &:hover {
      color: $error;
    }
  }

  &-content {
    @include text-ellipsis();
  }
}
</style>
