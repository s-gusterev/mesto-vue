<script setup>
const props = defineProps({
  card: Object,
  onClose: Function,
});
</script>

<template>
  <div
    class="popup popup_background_dark popup_type_picture root__popup"
    :class="{ popup_opened: props.card.isOpen }"
    @click="onClose"
    @keydown.esc="onClose"
  >
    <div class="popup__container" @click.stop>
      <img :src="props.card.link" :alt="props.card.name" class="popup__img" />
      <p class="popup__img-description">{{ props.card.name }}</p>
      <button
        class="popup__close"
        type="button"
        aria-label="Закрыть"
        @click="onClose"
      ></button>
    </div>
  </div>
</template>
