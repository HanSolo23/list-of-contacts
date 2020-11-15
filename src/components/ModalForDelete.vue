<template>
  <div class="modal__background">
    <div class="modal">
      <div class="modal__text">
        <slot name="modalText">Вы действительно хотите удалить этот пункт?</slot>
      </div>
      <div class="modal__buttons">
        <div class="modal__button_1">
          <slot name="firstModalButton">
            <button @click="removeField()" class="btn__yes">Да</button>
          </slot>
        </div>
        <div class="modal__button_2">
          <slot name="SecondModalButton">
            <button @click="close" class="btn__no">Нет</button>
          </slot>
        </div>
      </div>
    </div>
  </div>
  
</template>

<script>
export default {
  name: 'ModalForContactsList',
  methods: {
    close() {
      this.$emit('close-window');
    },
    removeField() {
      this.$emit('remove-field');
    }
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
    .modal__buttons {
      display: flex;
      justify-content: center;
      margin-top: 20px;
      padding: 20px;
      .modal__button_1 {
        margin-right: 50px;
      }
      .modal__button_2 {
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
    }
  }
}

</style>
