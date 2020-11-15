<template>
  <div id="app">
    <ContactsList 
      :contacts="contacts" 
      :firstPageTitle="firstPageTitle" 
      @push="addToContactsList" 
      @remove="removeFromContactsList" 
      @show-detail="showDetailInformation" 
      class="contacts"/>
    <ContactDetail 
      :secondPageTitle="secondPageTitle" 
      @push="addToListOfContactDetails" 
      class="contact__detail">
      <template #list v-if="isRender">
        <li v-for="(detail, index) in contacts[contactIndex].details" :key="index">
          {{ detail.firstField }} : {{ detail.secondField }}
          <button 
            @click="currentFieldIdForEdit = detail.id; 
            currentFieldIndex = index; showModalEditWindow(); 
            currentValueOfFirstField = detail.firstField; 
            currentValueOfSecondField = detail.secondField;
            isEdit = true"
            class="edit">
            <img :src="showImage(editImage)">
          </button>
          <button 
            @click="currentFieldIdForDelete = detail.id; showModalDeleteWindow(); isDelete = true" 
            class="delete">
            <img :src="showImage(deleteImage)">
          </button>
          <ModalForEdit 
            v-if="isModalEditVisible && currentFieldIdForEdit === detail.id" 
            @close-window="closeModalEditWindow"
            @edit="editContactDetails"/>
          <ModalForDelete 
            v-if="isModalDeleteVisible && currentFieldIdForDelete === detail.id" 
            @close-window="closeModalDeleteWindow"
            @remove-field="removeFromListOfContactDetails(index)"/>
        </li>
      </template>
      <template #buttonReturn>
        <button @click="returnChanges" class="btn__return">Отменить последние изменения!</button>
      </template>
      <template #buttonBack>
        <button @click="back" class="btn__back">Назад</button>
      </template>
    </ContactDetail>
  </div>
</template>

<script>
import ContactsList from './components/ContactsList.vue';
import ContactDetail from './components/ContactDetail.vue';
import ModalForDelete from './components/ModalForDelete.vue';
import ModalForEdit from './components/ModalForEdit.vue';

export default {
  name: 'App',
  components: {
    ContactsList,
    ContactDetail,
    ModalForDelete,
    ModalForEdit
  },
  data() {
    return {
      contacts: [
        { 
          id: 1, 
          lastName: 'Таран', 
          firstName: 'Иван', 
          details: [], 
        },
      ],
      firstPageTitle: 'Список контактов:',
      secondPageTitle: 'Детали контакта:',
      nextContactId: 2,
      // Contact index is neccesary for list of contact details render 
      contactIndex: 0,
      // Current field Ids are neccesary for control of modal windows render
      currentFieldIdForEdit: 0,
      currentFieldIdForDelete: 0,
      currentFieldIndex: 0,
      deleteImage: '4',
      editImage: '3',
      isModalDeleteVisible: false,
      isModalEditVisible: false,
      // Current value of fields in contact details are neccesary for return changes of edit or delete
      currentValueOfFirstField: '',
      currentValueOfSecondField: '',
      currentDeleteValue: '',
      isEdit: false,
      isDelete: false,
      isRender: true,
    }
  },
  methods: {
    addToContactsList(firstValue, secondValue) {
      this.contacts.push({
        id: this.nextContactId++,
        lastName: firstValue,
        firstName: secondValue,
        details: [],
      })
    },
    createRandomId() {
      return Math.floor(Math.random() * 1000)
    },
    addToListOfContactDetails(firstValue, secondValue) {
      this.contacts[this.contactIndex].details.push({
        id: this.createRandomId(),
        firstField: firstValue,
        secondField: secondValue
      })
    },
    removeFromContactsList(index) {
      this.isRender = false;
      this.contacts.splice(index, 1);
      for( let i = 0; i < this.contacts.length; i++) {
        this.contacts[i].id = i + 1;
      }
      this.nextContactId--;
    },
    removeFromListOfContactDetails(index) {
      this.currentDeleteValue = this.contacts[this.contactIndex].details[index];
      this.contacts[this.contactIndex].details.splice(index, 1);
      this.isEdit = false;
    },
    showDetailInformation(index) {
      this.isRender = true;
      let contacts = document.querySelector('.contacts');
      let details = document.querySelector('.contact__detail');
      contacts.style.transform = 'translateX(-6500px)';
      details.style.transform = 'translateX(0)';
      this.contactIndex = index;
    },
    editContactDetails(firstValue, secondValue) {
      this.contacts[this.contactIndex].details[this.currentFieldIndex].firstField = firstValue;
      this.contacts[this.contactIndex].details[this.currentFieldIndex].secondField = secondValue;
      this.isModalEditVisible = false;
    },
    returnChanges() {
      if(this.isEdit) {
        this.contacts[this.contactIndex].details[this.currentFieldIndex].firstField = this.currentValueOfFirstField;
        this.contacts[this.contactIndex].details[this.currentFieldIndex].secondField = this.currentValueOfSecondField;
      } else if(this.isDelete) {
        this.contacts[this.contactIndex].details.push(this.currentDeleteValue);
        this.isModalDeleteVisible = false;
      }
      this.isEdit = false;
      this.currentValueOfFirstField = '';
      this.currentValueOfSecondField = '';
      this.currentDeleteValue = '';
      this.isDelete = false;
    },
    showImage(element) {
      let imageName = element;
      return require(`./assets/${imageName}.png`);
    },
    showModalDeleteWindow() {
      this.isModalDeleteVisible = true;
    },
    closeModalDeleteWindow() {
      this.isModalDeleteVisible = false;
    },
    showModalEditWindow() {
      this.isModalEditVisible = true;
    },
    closeModalEditWindow() {
      this.isModalEditVisible = false;
    },
    back() {
      let contacts = document.querySelector('.contacts');
      let details = document.querySelector('.contact__detail');
      contacts.style.transform = 'translateX(0)';
      details.style.transform = 'translateX(-6500px)';
      this.currentValueOfFirstField = '';
      this.currentValueOfSecondField = '';
    }
  }
}
</script>

<style lang="scss">
@font-face {
  font-family: 'Montserrat';
  src: url('~@/assets/fonts/Montserrat-Regular.ttf') format('ttf'),
}
body {
  background-color: rgb(224, 222, 222);
  font-family: 'Montserrat', sans-serif;
  div {
    display: flex;
    justify-content: center;
    z-index: 1;
    .contacts {
      transition: all 1s ease;
    }
    .contact__detail {
      transform: translateX(-6500px);
      transition: all 1s ease;
      position: absolute;
      li {
        list-style-type: none;
        margin-bottom: 10px;
        .edit {
          background-color: rgb(255, 255, 255);
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
          left: 20px;
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
          left: 40px;
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
        .btn__no {
          width: 100px;
          height: 40px;
          background-color: rgb(7, 4, 4);
          color: white;
          border: 1px solid;
          cursor: pointer;
          outline: none;
          box-shadow: 2px 2px 10px 1px;
          &:hover {
            opacity: 0.9
          }
        }
      }
      .btn__back {
        margin-top: 20px;
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
      .btn__return {
        margin-top: 20px;
        width: 150px;
        height: 50px;
        background-color: rgb(207, 47, 47);
        border: 1px solid;
        cursor: pointer;
        outline: none;
        box-shadow: 2px 2px 10px 1px;
        transition: all 1s ease;
        margin-left: 190px;
        &:hover {
          background-color: rgb(49, 97, 99);
        }
      }
    }
  }
}
</style>
