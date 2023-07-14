<template>
  <div class="flex mt-2">
    <ui-button
      type="primary"
      label="Zmień nazwę"
      @click-button="openChangeNameModal()"
    />
    <ui-button
      classes="ml-2"
      type="secondary"
      label="Zmień hasło"
      @click-button="openChangePasswordModal()"
    />
    <ui-button
      classes="ml-2"
      type="danger"
      label="Usuń konto"
      @click-button="openDeleteProfileModal()"
    />

    <ui-modal
      :title="modalTitle"
      :is-open="isModalOpen"
      @close-modal="closeModal()"
    >
      <template v-if="modalName === 'name'">
        <ui-input
          :value="name"
          label="Nowa nazwa użytkownika"
          placeholder="Wprowadź nazwę użytkownika"
          @value-change="name = $event"
        />
        <ui-button
          classes="mt-2 m-auto"
          label="Zmień nazwę"
          :loading="loading"
          @click-button="chageUserData('name')"
        />
      </template>

      <template v-else-if="modalName === 'password'">
        <ui-input
          type="password"
          :value="password"
          label="Nowe hasło użytkownika"
          placeholder="Wprowadź hasło użytkownika"
          @value-change="password = $event"
        />
        <ui-input
          type="password"
          :value="passwordConfirmation"
          label="Potwierdzenie hasła użytkownika"
          placeholder="Wprowadź hasło użytkownika jeszcze raz"
          @value-change="passwordConfirmation = $event"
        />
        <ui-button
          classes="mt-2 m-auto"
          label="Zmień hasło"
          :loading="loading"
          @click-button="chageUserData('password')"
        />
      </template>

      <template v-else-if="modalName === 'delete'">
        <ui-typography type="title">
          Uwaga - potwierdzasz USUNIĘCIE tego konta!
        </ui-typography>
        <ui-typography type="text">
          Tej akcji nie da się odwrócić! Jeżeli nie chcesz tego robić, po prostu
          zamknij to okno.
        </ui-typography>
        <ui-button
          classes="mt-2 m-auto"
          type="danger"
          label="Usuń konto"
          :loading="loading"
          @click-button="deleteUser()"
        />
      </template>

      <ui-error-message v-if="error" :message="error" />
    </ui-modal>
  </div>
</template>

<script lang="ts">
import { UpdateUserDto } from "interfaces/UpdateUser.dto";

export default {
  data() {
    return {
      isModalOpen: false,
      modalName: "" as "name" | "password" | "delete" | "",
      name: "",
      password: "",
      passwordConfirmation: "",
      loading: false,
      error: "" as string | string[],
    };
  },
  computed: {
    modalTitle(): string {
      switch (this.modalName) {
        case "name":
          return "Zmień nazwę użytkownika";
        case "password":
          return "Zmień hasło";
        case "delete":
          return "Usuń dane konto";
        default:
          return "";
      }
    },
  },
  methods: {
    openChangeNameModal(): void {
      this.modalName = "name";
      this.isModalOpen = true;
    },
    openChangePasswordModal(): void {
      (this.modalName = "password"), (this.isModalOpen = true);
    },
    openDeleteProfileModal(): void {
      (this.modalName = "delete"), (this.isModalOpen = true);
    },
    closeModal(): void {
      this.name = "";
      this.password = "";
      this.passwordConfirmation = "";
      this.error = "";
      this.modalName = "";
      this.isModalOpen = false;
    },
    async chageUserData(field: "name" | "password"): Promise<void> {
      if (this.password !== this.passwordConfirmation) {
        this.error = "Hasła się różnią.";
        return;
      }

      const body: UpdateUserDto = {};
      if (field === "name") {
        body.name = this.name;
      }
      if (field === "password") {
        body.password = this.password;
      }

      try {
        this.loading = true;

        if (this.token) {
          await this.updateUserApi(body, this.token);
        }

        await this.setUserState();
        this.closeModal();
      } catch (err: any) {
        this.error =
          err.data?.message || "Nie udało się zmienić danych użytkownika.";
      } finally {
        this.loading = false;
      }
    },
    async deleteUser(): Promise<void> {
      try {
        this.loading = true;

        if (this.token) {
          await this.deleteUserApi(this.token);
        }

        await this.setUserState();
        this.closeModal();
      } catch (err: any) {
        this.error =
          err.data?.message || "Nie udało się usunąć konto użytkownika.";
      } finally {
        this.loading = false;
      }
    },
  },
  setup() {
    const { token, setUserState } = useAppState();
    const { updateUserApi, deleteUserApi } = useUserApi();

    return {
      token,
      setUserState,
      updateUserApi,
      deleteUserApi,
    };
  },
};
</script>
