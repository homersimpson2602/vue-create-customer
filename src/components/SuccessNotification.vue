<template>
<div class="success-notification" 
      v-if="completed" 
    >
      <h3>Клиент успешно создан</h3>
      <div class="success-icon">
        <img alt="success_icon" src=".././assets/check.svg">
      </div>
      <ul>
        <li> Имя: <span>{{ surname + ' ' + name + ' ' + patronymic }}</span></li>
        <li> Телефон: <span>{{ phoneNum }} </span></li>
      </ul>
      
      <button id="back-to-create"
        @click="backToCreate"
      >К созданию клиента</button>
    </div>
  
</template>

<script>

export default {
  name: 'SuccessNotification',
  props: {
    completed: Boolean,
    surname: String,
    name: String,
    patronymic: String,
    phoneNum: String,
    backToCreate: Function
  }
}
</script>

<style scoped lang="sass">
.success-notification
  position: relative
  width: 360px
  height: 400px
  box-shadow: 0px 1px 3px 0px #ccc
  border-radius: 5px
  margin: 0 auto
  display: flex
  flex-direction: column
  align-items: center
  justify-content: space-evenly
  
  ul
    list-style-type: none
    align-self: start
    max-width: 80%

    li
      margin-bottom: 10px

    span
      font-weight: 700

  .success-icon
    position: absolute
    top: 10px

  img
    width: 50px

  button
    padding: 10px
    border: none
    font: inherit
    color: inherit
    background-color: transparent
    background-color: #52B152
    color: #fff
    font-size: 1.2rem
    outline: none
    cursor: pointer

    &:hover
      transform: translateY(1px)
      filter: saturate(150%)

</style>
