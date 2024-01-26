<script setup>
defineProps({
  name: String,
  title: String,
  btnText: String,
  isOpen: Boolean,
});

const emit = defineEmits(["onClose", "onSubmit"]);

const handleSubmit = (e) => {
  e.preventDefault();
  emit("onSubmit");
};
</script>

<template>
  <div
    :class="`popup ${
      isOpen ? 'popup_opened' : ''
    } popup_background_light popup_type_${name} root__popup`"
  >
    <form
      class="popup__container popup__container_type_form"
      :name="name"
      @submit="handleSubmit"
    >
      <h2 class="popup__title">{{ title }}</h2>

      <slot></slot>

      <button class="popup__btn root__btn" type="submit">
        {{ btnText }}
      </button>
      <button
        class="popup__close"
        type="button"
        aria-label="Закрыть"
        @click="emit('onClose')"
      ></button>
    </form>
  </div>
</template>
