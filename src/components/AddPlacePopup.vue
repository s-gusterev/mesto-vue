<script setup>
import { ref } from "vue";
import PopupWithForm from "./PopupWithForm.vue";
defineProps({
  isOpen: Boolean,
});

const name = ref("");
const link = ref("");
const emit = defineEmits(["onSubmit", "onClose"]);
const handleSubmit = () => {
  emit("onSubmit", { name: name.value, link: link.value });
  name.value = "";
  link.value = "";
};
</script>
<template>
  <PopupWithForm
    :is-open="isOpen"
    title="Новое место"
    btn-text="Cоздать"
    name="card-add"
    @on-close="emit('onClose')"
    @on-submit="handleSubmit"
  >
    <label class="popup__label" for="input-place">
      <input
        class="popup__input"
        type="text"
        name="place"
        id="input-place"
        required
        placeholder="Название"
        min-length="2"
        max-length="30"
        value="{name}"
        v-model="name"
      />
      <span class="popup__input-error input-place-error"></span>
    </label>
    <label class="popup__label" for="input-image">
      <input
        class="popup__input"
        type="url"
        name="image"
        id="input-image"
        required
        placeholder="Ссылка на картинку"
        value="{link}"
        v-model="link"
      />
      <span class="popup__input-error input-image-error"></span>
    </label>
  </PopupWithForm>
</template>
