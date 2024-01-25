<script setup>
import { inject, ref } from "vue";

const props = defineProps({
  card: {
    type: Object,
    required: true,
  },
});

const emit = defineEmits(["likeCard", "deleteCard", "openImage"]);

const user = inject("currentUser");
const isOwnerUser = props.card.owner._id === user._id;

const cardDeleteButtonClassName = `card__del ${
  isOwnerUser ? "card__del_visible" : "card__del_hidden"
}`;

const isLiked = ref(props.card.likes.some((i) => i._id === user._id));

const handleCardlike = () => {
  isLiked.value = !isLiked.value;
  emit("likeCard", props.card);
};

const handleCardDelete = () => {
  emit("deleteCard", props.card);
};

const handleCardClick = () => {
  emit("openImage", props.card);
};
</script>
<template>
  <li class="card">
    <div class="card__text">
      <h2 class="card__title">{{ props.card.name }}</h2>
      <div class="card__like-section">
        <button
          :class="isLiked ? 'card__like card__like_active' : 'card__like'"
          type="button"
          @click="handleCardlike"
        ></button>
        <span class="card__like-info">{{ props.card.likes.length }}</span>
      </div>
    </div>
    <div
      class="card__img"
      :style="{ backgroundImage: `url(${props.card.link})` }"
      @click="handleCardClick"
    ></div>
    <button
      :class="cardDeleteButtonClassName"
      type="button"
      @click="handleCardDelete"
    ></button>
  </li>
</template>
