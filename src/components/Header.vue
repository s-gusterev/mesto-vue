<script setup>
import logo from "../images/logo.svg";
</script>

<template>
  <header className="header root__header">
    <a href="/" className="logo header__logo"
      ><img :src="logo" alt="Логотип" className="logo__img"
    /></a>
  </header>
</template>
