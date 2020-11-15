<template>
  <div class="main">
    <div class="page__title">{{ firstPageTitle }}</div>
    <ul class="contacts__list">
      <li v-for="(contact, index) in contacts" :key="index">
        <button class="details" @click="showDetails(index)">
          {{ contact.id }} : {{ contact.lastName }} {{ contact.firstName }}
        </button>
        <button 
          @click="currentIndex = contact.id; showModalWindow()" 
          class="delete">
          <img :src="showImage(deleteImage)">
        </button>
        <ModalForDelete 
          v-if="isVisible && currentIndex === contact.id" 
          @close-window="closeModalWindow">
          <template #modalText>
            Вы действительно хотите удалить контакт?
          </template>
          <template #firstModalButton>
            <button @click="removeContact(index)" class="btn__yes">Да</button>
          </template>
        </ModalForDelete>
      </li>  
    </ul>
    <form v-on:keyup.enter="submit" class="add__contact">
      <div class="form__title">Добавить контакт:</div>
      <div class="form__inputs">
        <div class="form__input">
          <label for="input__last__name">Фамилия</label>
          <input 
            id="input__last__name" 
            type="text"
            v-model="lastName"  
            v-on:keyup.enter="addContact(lastName, firstName)">
        </div>
        <div class="form__input">
          <label for="input__first__name">Имя</label>
          <input 
            id="input__first__name" 
            type="text" 
            v-model="firstName"  
            v-on:keyup.enter="addContact(lastName, firstName)">
        </div>
      </div>
      <button 
        class="add__contact__button" 
        type="button" 
        @click="addContact(lastName, firstName)">
        Добавить
      </button>
    </form>
  </div>
</template>

<script>
import ModalForDelete from '../components/ModalForDelete.vue'

export default {
  name: 'ContactsList',
  props: ['contacts', 'firstPageTitle'],
  data() {
    return {
      lastName: '',
      firstName: '',
      isVisible: false,
      currentIndex: 0,
      deleteImage: '4',
    }
  },
  methods: {
    addContact(firstValue, secondValue) {
      this.$emit('push', firstValue, secondValue);
      this.lastName = '';
      this.firstName = '';
    },
    removeContact(index) {
      this.$emit('remove', index);
      this.isVisible = false;
    },
    showImage(element) {
      let imageName = element;
      return require(`../assets/${imageName}.png`);
    },
    showDetails(index) {
      this.$emit('show-detail', index);
    },
    showModalWindow() {
      this.isVisible = true;
    },
    closeModalWindow() {
      this.isVisible = false;
    },
  },
  components: {
    ModalForDelete,
  },
}
</script>


<style scoped lang="scss">
.main {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 25%;
  border: 2px solid;
  border-radius: 1%;
  padding: 20px;
  background-color: rgb(213, 216, 164);
  box-shadow: 2px 2px 20px 1px;
  .contacts__list {
    li {
      list-style-type: none;
      margin-bottom: 10px;
      position: relative;
      .details {
        transition: all 1s ease;
        background-color: rgb(135, 202, 161);
        border: 1px solid;
        transition: all 1s ease;
        position: relative;
        right: 20px;
        outline: none;
        font-size: 18px;
        padding: 10px;
        box-shadow: 2px 2px 10px 1px;
        cursor: pointer;
        &:hover {
          background-color: rgb(49, 97, 55);
        }
      }
      .delete {
        background-color: rgb(224, 89, 89);
        border: none;
        transition: all 1s ease;
        position: relative;
        top: 10px;
        outline: none;
        cursor: pointer;
        width: 29px;
        height: 29px;
        box-shadow: 2px 2px 10px 1px;
        border-radius: 50%;
        img {
          width: 30px;
          height: 30px;
          position: relative;
          left: -6.5px;
          top: -1.5px;
        }
        &:hover {
          transform: scale(1.1)
        }
      }
      .btn__yes {
        width: 100px;
        height: 40px;
        background-color: rgb(124, 40, 40);
        border: 1px solid;
        cursor: pointer;
        outline: none;
        box-shadow: 2px 2px 10px 1px;
        &:hover {
          opacity: 0.9
        }
      }
    }
  }
  .add__contact {
    .form__title {
      margin-bottom: 10px;
    }
    .form__inputs {
      display: flex;
      .form__input {
        display: flex;
        flex-direction: column;
        margin-bottom: 10px;
        input {
          background-color:  rgb(156, 189, 125);
          border: 1px solid;
          box-shadow: 2px 2px 10px 1px;
        }
        &:nth-child(1) {
          margin-right: 10px;
        }  
      }
    }
    .add__contact__button {
      width: 100px;
      height: 40px;
      background-color: rgb(137, 213, 216);
      border: 1px solid;
      cursor: pointer;
      outline: none;
      box-shadow: 2px 2px 10px 1px;
      transition: all 1s ease;
      &:hover {
        background-color: rgb(49, 97, 99);
      }
    }
  }
}
@media(max-width: 1400px) {
  .main {
    width: 30%
  }
}
@media(max-width: 1200px) {
  .main {
    width: 45%
  }
}
@media(max-width: 1000px) {
  .main {
    width: 55%
  }
}
@media(max-width: 800px) {
  .main {
    width: 65%
  }
}
@media(max-width: 600px) {
  .main {
    width: 75%
  }
}
@media(max-width: 400px) {
  .main {
    width: 85%
  }
}
</style>
