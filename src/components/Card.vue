<script setup></script>
<template>
  <li class="card">
    <div class="card__text">
      <h2 class="card__title">{card.name}</h2>
      <div class="card__like-section">
        <button class="card__like" type="button"></button>
        <span class="card__like-info">{card.likes.length}</span>
      </div>
    </div>
    <div class="card__img" style=""></div>
    <button class="card__del card__del_visible" type="button"></button>
  </li>
</template>
