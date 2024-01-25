<script setup>
import { reactive, ref, provide, watch, onMounted } from "vue";
import api from "./utils/api.js";
import Header from "./components/Header.vue";
import Main from "./components/Main.vue";
import Footer from "./components/Footer.vue";
import ImagePopup from "./components/ImagePopup.vue";

const currentUser = reactive({
  name: "",
  about: "",
  avatar: "",
  _id: "",
});

const cards = ref([]);

const selectedCard = ref({
  isOpen: false,
  link: "",
  name: "",
});

const getUser = async () => {
  try {
    const user = await api.getProfile();
    currentUser.name = user.name;
    currentUser.about = user.about;
    currentUser.avatar = user.avatar;
    currentUser._id = user._id;
  } catch (error) {
    console.log(error);
  }
};

const getCards = async () => {
  try {
    const initialCards = await api.getInitialCards();
    cards.value = initialCards;
  } catch (error) {
    console.log(error);
  }
};

const handleCardLike = async (card) => {
  try {
    const isLiked = card.likes.some((i) => i._id === currentUser._id);
    const likeCard = await api.changeLikeCardStatus(card._id, !isLiked);
    cards.value = cards.value.map((c) => (c._id === card._id ? likeCard : c));
  } catch (error) {
    console.log(error);
  }
};

const handleCardDelete = async (card) => {
  try {
    await api.delCard(card._id);
    cards.value = cards.value.filter((c) => c._id !== card._id);
  } catch (error) {
    console.log(error);
  }
};

const handleCardClick = (card) => {
  selectedCard.value = {
    isOpen: true,
    name: card.name,
    link: card.link,
  };
};

onMounted(async () => {
  await getUser();
  await getCards();
});

provide("currentUser", currentUser);
provide("cards", cards);
</script>

<template>
  <div class="root__container">
    <Header />
    <Main
      @like-card="handleCardLike"
      @delete-card="handleCardDelete"
      @open-image="handleCardClick"
    />
    <Footer />
    <ImagePopup
      :card="selectedCard"
      :on-close="() => (selectedCard.isOpen = false)"
    />
  </div>
</template>
