<script setup>
import { reactive, ref, provide, watch, onMounted } from "vue";
import api from "./utils/api.js";
import Header from "./components/Header.vue";
import Main from "./components/Main.vue";
import Footer from "./components/Footer.vue";
import ImagePopup from "./components/ImagePopup.vue";
import EditAvatarPopup from "./components/EditAvatarPopup.vue";
import AddPlacePopup from "./components/AddPlacePopup.vue";
import EditProfilePopup from "./components/EditProfilePopup.vue";
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
const isAddPlacePopupOpen = ref(false);
const isEditProfilePopupOpen = ref(false);

// открытие и закрытие попапов--------
const openEditAvatarPopup = () => {
  isEditAvatarPopupOpen.value = true;
};

const openAddPlacePopup = () => {
  isAddPlacePopupOpen.value = true;
};

const openEditProfilePopup = () => {
  isEditProfilePopupOpen.value = true;
};

const closeAllPopups = () => {
  selectedCard.value.isOpen = false;
  isEditAvatarPopupOpen.value = false;
  isAddPlacePopupOpen.value = false;
  isEditProfilePopupOpen.value = false;
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

// обновление аватара
const handleUpdateAvatar = async (avatar) => {
  try {
    const updateAvatar = await api.updateAvatar(avatar);
    currentUser.avatar = updateAvatar.avatar;
    closeAllPopups();
  } catch (error) {
    console.log(err);
  }
};

// добавление новой карточки
const handleAddPlaceSubmit = async (card) => {
  try {
    const { name, link } = card;

    const addPlace = await api.addCard(name, link);
    cards.value = [addPlace, ...cards.value];
    closeAllPopups();
  } catch (error) {
    console.log(error);
  }
};

// обновление профиля
const handleUpdateUser = async (user) => {
  try {
    const { name, about } = user;

    const updateUser = await api.editProfile(name, about);
    currentUser.name = updateUser.name;
    currentUser.about = updateUser.about;
    closeAllPopups();
  } catch (error) {
    console.log(error);
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
      @open-add-place="openAddPlacePopup"
      @open-edit-profile="openEditProfilePopup"
    />
    <Footer />
    <ImagePopup :card="selectedCard" :on-close="closeAllPopups" />

    <EditAvatarPopup
      :is-open="isEditAvatarPopupOpen"
      @on-close="closeAllPopups"
      @on-submit="handleUpdateAvatar"
    />
    <AddPlacePopup
      :is-open="isAddPlacePopupOpen"
      @on-close="closeAllPopups"
      @on-submit="handleAddPlaceSubmit"
    />

    <EditProfilePopup
      :is-open="isEditProfilePopupOpen"
      @on-close="closeAllPopups"
      @on-submit="handleUpdateUser"
    />
  </div>
</template>
