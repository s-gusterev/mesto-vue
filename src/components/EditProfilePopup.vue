<script setup>
import { ref, inject, watch } from "vue";
import PopupWithForm from "./PopupWithForm.vue";
defineProps({
  isOpen: Boolean,
});

const user = inject("currentUser");

const name = ref("");
const about = ref("");
const emit = defineEmits(["onSubmit", "onClose"]);
const handleSubmit = () => {
  emit("onSubmit", { name: name.value, about: about.value });
};

watch(user, () => {
  name.value = user.name;
  about.value = user.about;
});
</script>

<template>
  <PopupWithForm
    :is-open="isOpen"
    title="Редактировать профиль"
    btn-text="Сохранить"
    name="edit-profile"
    @on-close="emit('onClose')"
    @on-submit="handleSubmit"
  >
    <label class="popup__label" for="input-name">
      <input
        class="popup__input"
        type="text"
        name="name"
        id="input-name"
        required
        placeholder="Имя"
        min-length="2"
        max-length="40"
        v-model="name"
      />
      <span class="popup__input-error input-name-error"></span>
    </label>
    <label class="popup__label" for="input-about">
      <input
        class="popup__input"
        type="text"
        name="about"
        id="input-about"
        required
        placeholder="Род деятельности"
        min-length="2"
        max-length="200"
        v-model="about"
      />
      <span class="popup__input-error input-about-error"></span>
    </label>
  </PopupWithForm>
</template>
