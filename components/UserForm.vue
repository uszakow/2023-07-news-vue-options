<template>
  <div class="user-form relative m-auto mt-3">
    <ui-tabs
      :tabs="tabs"
      :active-tab="activeTab"
      @tab-change="activeTab = $event"
    />
    <div class="user-form-wrap px-2 py-3">
      <ui-typography type="title">
        {{ activeTab === "login" ? "Dane użytkownika:" : "Nowy użytkownik:" }}
      </ui-typography>
      <ui-input
        :value="name"
        label="Nazwa użytkownika"
        placeholder="Wprowadź nazwę użytkownika"
        required
        @value-change="name = $event"
      />
      <ui-input
        type="password"
        :value="password"
        label="Hasło"
        :placeholder="
          activeTab === 'login'
            ? 'Wprowadź hasło'
            : 'Hasło ma zawierać co najmniej 4 znaki'
        "
        required
        @value-change="password = $event"
      />
      <ui-button
        classes="mt-2 mb-1 m-auto"
        :label="activeTab === 'login' ? 'Zaloguj się' : 'Utwórz konto'"
        :loading="loading"
        @click-button="activeTab === 'login' ? loginUser() : createUser()"
      />
      <ui-error-message v-if="error" :message="error" />
    </div>
  </div>
</template>

<script lang="ts">
import { TabItemInterface } from "interfaces/TabItem.interface";
import { useUserApi } from "@/composables/useUserApi";
import { UserDto } from "interfaces/User.dto";

export default {
  data() {
    return {
      tabs: [
        { id: "login", label: "Zaloguj się" },
        { id: "registration", label: "Utwórz konto" },
      ] as TabItemInterface[],
      activeTab: "",
      name: "",
      password: "",
      loading: false,
      error: "" as string | string[],
      timeout: null as NodeJS.Timeout | null,
    };
  },
  watch: {
    error() {
      if (this.timeout) {
        clearTimeout(this.timeout);
      }

      this.timeout = setTimeout(() => {
        this.error = "";
      }, 5000);
    },
  },
  methods: {
    async loginUser(): Promise<void> {
      const body: UserDto = {
        name: this.name,
        password: this.password,
      };

      try {
        this.loading = true;
        const token = await this.loginUserApi(body);

        if (token) {
          localStorage.setItem("token", token);
        }

        await this.setUserState();
      } catch (err: any) {
        this.error =
          err.data?.message ||
          "Nie udało się zalogować, prosimy spróbować później";
      } finally {
        this.loading = false;
      }
    },
    async createUser(): Promise<void> {
      const body: UserDto = {
        name: this.name,
        password: this.password,
      };

      try {
        this.loading = true;

        const response = await this.createUserApi(body);

        if (response.statusCode === 201) {
          await this.loginUser();
        }
      } catch (err: any) {
        this.error =
          err.data?.message || "Nie udało się stworzyć konto użytkownika";
      } finally {
        this.loading = false;
      }
    },
  },
  mounted() {
    this.activeTab = this.tabs[0].id;
  },
  beforeUnmount() {
    if (this.timeout) {
      clearTimeout(this.timeout);
    }
  },
  setup() {
    const { setUserState } = useAppState();
    const { createUserApi, loginUserApi } = useUserApi();

    return {
      setUserState,
      createUserApi,
      loginUserApi,
    };
  },
};
</script>

<style lang="scss" scoped>
@import "styles/variables";
@import "styles/functions";

.user-form {
  width: px(600);

  &-wrap {
    border-right: 2px solid $main;
    border-bottom: 2px solid $main;
    border-left: 2px solid $main;
    border-bottom-right-radius: px(22);
    border-bottom-left-radius: px(22);
  }
}
</style>
