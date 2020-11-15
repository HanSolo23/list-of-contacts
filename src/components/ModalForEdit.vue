<template>
  <div class="modal__background">
    <div class="modal">
      <div class="modal__text">
        <slot name="modalText">Редактирование поля:</slot>
      </div>
      <div class="modal__form">
        <slot name="form">
          <form>
            <div class="edit__first__field">
              <slot name="firstInput">
                <label for="edit__first__field">Название:</label>
                <input id="edit__first__field" type="text" v-model="firstField" value="текст">
              </slot>
            </div>
            <div class="edit__second__field">
              <slot name="secondInput">
                <label for="edit__second__field">Coдержание:</label>
                <input id="edit__second__field" type="text" v-model="secondField">
              </slot>
            </div>
            <div class="modal__buttons">
              <div class="modal__button_1">
                <slot name="firstModalButton">
                  <button 
                    @click="editContact(firstField, secondField)" 
                    class="btn__edit" 
                    type="button">
                    Редактировать!
                  </button>
                </slot>
              </div>
              <div class="modal__button_2">
                <slot name="SecondModalButton">
                  <button  @click="close" class="btn__no">Назад</button>
                </slot>
              </div>
            </div>
          </form>
        </slot>
      </div>
    </div>
  </div>
  
</template>

<script>
export default {
  name: 'ModalForEdit',
  data() {
    return {
      firstField: '',
      secondField: '',
    }
  },
  methods: {
    editContact(firstValue, secondValue) {
      this.$emit('edit', firstValue, secondValue);
      this.firstField = '';
      this.secondField = '';
    },
    close() {
      this.$emit('close-window');
    },
  }
}
</script>


<style scoped lang="scss">
.modal__background {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.3);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 100;
  .modal {
    background: #b8abab;
    box-shadow: 2px 2px 20px 1px;
    overflow-x: auto;
    display: flex;
    flex-direction: column;
    .modal__text {
      padding: 20px
    }
    .modal__form {
      .edit__first__field {
        margin-bottom: 10px;
        label {
          margin-right: 27px;
        }
      }
      .edit__second__field {
        label {
          margin-right: 5px;
        }
      }
      .modal__buttons {
        display: flex;
        justify-content: center;
        margin-top: 20px;
        padding: 20px;
        .modal__button_1 {
          margin-right: 50px;
          .btn__edit {
            width: 115px;
            height: 40px;
            background-color: rgb(137, 213, 216);
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
    }
  }
}

</style>
