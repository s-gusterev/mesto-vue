<script setup>
import { ref } from "vue";
import PopupWithForm from "./PopupWithForm.vue";

defineProps({
  isOpen: Boolean,
});

const avatar = ref("");

const emit = defineEmits(["onSubmit", "onClose"]);

const handleSubmit = () => {
  console.log("onSubmit");
  emit("onSubmit", avatar.value);
  avatar.value = "";
};
</script>

<template>
  <PopupWithForm
    :is-open="isOpen"
    title="Обновить аватар"
    btn-text="Обновить"
    name="edit-avatar"
    @on-close="emit('onClose')"
    @on-submit="handleSubmit"
  >
    <label className="popup__label" for="input-avatar">
      <input
        v-model="avatar"
        class="popup__input"
        type="url"
        name="avatar"
        id="input-avatar"
        required
        placeholder="Ссылка на аватар"
      />
      <span class="popup__input-error input-avatar-error"></span>
    </label>
  </PopupWithForm>
</template>
