<script setup>
import { inject } from "vue";
import Card from "./Card.vue";
const user = inject("currentUser");
const cards = inject("cards");

const emit = defineEmits([
  "likeCard",
  "deleteCard",
  "openImage",
  "openEditAvatar",
  "openAddPlace",
  "openEditProfile",
]);
</script>
<template>
  <main class="main">
    <section class="profile main__profile">
      <div class="profile__info">
        <div
          class="profile__avatar"
          :style="{ backgroundImage: `url(${user.avatar})` }"
        >
          <button
            class="profile__btn-edit-avatar"
            type="button"
            aria-label="Редактировать аватар"
            @click="emit('openEditAvatar')"
          ></button>
        </div>
        <div class="profile__name">
          <h1 class="profile__title">{{ user.name }}</h1>
          <p class="profile__subtitle">{{ user.about }}</p>
          <button
            class="profile__btn-edit-profile"
            type="button"
            aria-label="Редактировать данные профиля"
            @click="emit('openEditProfile')"
          ></button>
        </div>
        <button
          class="profile__btn-add-place"
          type="button"
          aria-label="Добавить место"
          @click="emit('openAddPlace')"
        ></button>
      </div>
      <ul class="cards">
        <Card
          v-for="card in cards"
          :key="card._id"
          :card="card"
          @like-card="emit('likeCard', card)"
          @delete-card="emit('deleteCard', card)"
          @open-image="emit('openImage', card)"
        />
      </ul>
    </section>
  </main>
</template>
