<script setup>
import { reactive, ref, provide, watch, onMounted } from "vue";
import api from "./utils/api.js";
import Header from "./components/Header.vue";
import Main from "./components/Main.vue";
import Footer from "./components/Footer.vue";
import ImagePopup from "./components/ImagePopup.vue";
import EditAvatarPopup from "./components/EditAvatarPopup.vue";

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

const isEditAvatarPopupOpen = ref(false);

// открытие и закрытие попапов--------
const openEditAvatarPopup = () => {
  isEditAvatarPopupOpen.value = true;
};

const closeAllPopups = () => {
  selectedCard.value.isOpen = false;
  isEditAvatarPopupOpen.value = false;
};
// открытие и закрытие попапов--------

// получение текущего пользователя
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
// получение карточек
const getCards = async () => {
  try {
    const initialCards = await api.getInitialCards();
    cards.value = initialCards;
  } catch (error) {
    console.log(error);
  }
};
// отметка карточки лайком
const handleCardLike = async (card) => {
  try {
    const isLiked = card.likes.some((i) => i._id === currentUser._id);
    const likeCard = await api.changeLikeCardStatus(card._id, !isLiked);
    cards.value = cards.value.map((c) => (c._id === card._id ? likeCard : c));
  } catch (error) {
    console.log(error);
  }
};
// удаление карточки
const handleCardDelete = async (card) => {
  try {
    await api.delCard(card._id);
    cards.value = cards.value.filter((c) => c._id !== card._id);
  } catch (error) {
    console.log(error);
  }
};
// клик по карточке (открытие попапа содержимого карточки)
const handleCardClick = (card) => {
  selectedCard.value = {
    isOpen: true,
    name: card.name,
    link: card.link,
  };
};

const handleUpdateAvatar = async (avatar) => {
  try {
    const updateAvatar = await api.updateAvatar(avatar);
    currentUser.avatar = updateAvatar.avatar;
    closeAllPopups();
  } catch (error) {
    console.log(err);
  }
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
      @open-edit-avatar="openEditAvatarPopup"
    />
    <Footer />
    <ImagePopup :card="selectedCard" :on-close="closeAllPopups" />

    <EditAvatarPopup
      :is-open="isEditAvatarPopupOpen"
      @on-close="closeAllPopups"
      @on-submit="handleUpdateAvatar"
    />
  </div>
</template>
