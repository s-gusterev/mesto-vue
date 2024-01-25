<script setup>
import Card from "./Card.vue";
</script>
<template>
  <main class="main">
    <section class="profile main__profile">
      <div class="profile__info">
        <div
          class="profile__avatar"
          style="
            background-image: url('https://pictures.s3.yandex.net/resources/jacques-cousteau_1604399756.png');
          "
        >
          <button
            class="profile__btn-edit-avatar"
            type="button"
            aria-label="Редактировать аватар"
          ></button>
        </div>
        <div class="profile__name">
          <h1 class="profile__title">{user.name}</h1>
          <p class="profile__subtitle">{user.about}</p>
          <button
            class="profile__btn-edit-profile"
            type="button"
            aria-label="Редактировать данные профиля"
          ></button>
        </div>
        <button
          class="profile__btn-add-place"
          type="button"
          aria-label="Добавить место"
        ></button>
      </div>
      <ul class="cards">
        <Card />
        <Card />
        <Card />
      </ul>
    </section>
  </main>
</template>
