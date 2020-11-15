<template>
  <div class="main">
    <div class="page__title">{{ secondPageTitle }}</div>
    <ul class="contact__details">
      <slot name="list"></slot>
    </ul>
    <form class="add__field">
      <div class="form__title">Добавить поле:</div>
      <div class="form__inputs">
        <div class="form__input">
          <label for="input__field__name_1">Название</label>
          <input 
            id="input__field__name_1" 
            type="text" 
            v-model="firstField"  
            v-on:keyup.enter="addNewField(firstField, secondField)">
        </div>
        <div class="form__input">
          <label for="input__field__name_2">Содерджание</label>
          <input 
            id="input__field__name_2" 
            type="text" v-model="secondField"  
            v-on:keyup.enter=" addNewField(firstField, secondField)">
        </div>
      </div>
      <button 
        class="add__field__button" 
        type="button" 
        @click=" addNewField(firstField, secondField)">
        Добавить
      </button>
    </form>
    <div class="button__return">
        <slot name="buttonReturn"></slot>
    </div>
    <div class="button__back">
        <slot name="buttonBack"></slot>
    </div>
  </div>
</template>

<script>

export default {
  name: 'ContactDetail',
  props: ['secondPageTitle'],
  data() {
    return {
      firstField: '',
      secondField: '',
      deleteImage: '1',
    }
  },
  methods: {
    addNewField(firstValue, secondValue) {
      this.$emit('push', firstValue, secondValue);
      this.firstField = '';
      this.secondField = '';
    },
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
  transition: all 1s ease;
  .add__field {
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
    .add__field__button {
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
