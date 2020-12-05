<template>
  <div id="app">
    <transition name="fade">
    <h1
      v-if="!completed" 
    >Создать клиента</h1>
    </transition>
    <transition name="fade">
    <form
       v-if="!completed"
    >
      <div class="main-section">
        <h2>Общая информация</h2>
        <div class="form__item">
          <label for="surname" class="decription required">Фамилия</label>
          <span class="error-notification"
            v-show=" !$v.mainSection.surname.$model && $v.mainSection.surname.$dirty "
          >Обязательное поле</span>
          <span class="error-notification" 
            v-if="!$v.mainSection.surname.minLength && $v.mainSection.surname.$dirty"
          >
            Хотя бы 
            {{$v.mainSection.surname.$params.minLength.min}} буквы
          </span>
          <input id="surname" type="text"
            :class="{ 'error-highlight' : $v.mainSection.surname.$error,
              'success-hightlight' : !$v.mainSection.surname.$error 
              && !$v.mainSection.surname.$invalid }"
            v-model.trim="$v.mainSection.surname.$model" placeholder="Иванов"
          >
        </div>

        <div class="form__item ">
          <label for="name" class="decription required">Имя</label>
          <span class="error-notification"
            v-show=" !$v.mainSection.name.$model && $v.mainSection.name.$dirty"
          >Обязательное поле</span>

          <span class="error-notification" 
            v-if="!$v.mainSection.name.minLength && $v.mainSection.name.$dirty"
          >Хотя бы 
            {{$v.mainSection.name.$params.minLength.min}} буквы 
          </span>

          <input id="name" type="text"
            :class="{ 'error-highlight' : $v.mainSection.name.$error,
              'success-hightlight' : !$v.mainSection.name.$error && !$v.mainSection.name.$invalid  }"
            v-model.trim="$v.mainSection.name.$model" placeholder="Иван"
          >
        </div>

        <div class="form__item">
          <label for="patronomic" class="decription">Отчество</label>
          <span class="error-notification" 
            v-if="!$v.mainSection.patronymic.minLength && $v.mainSection.patronymic.$dirty">
            Хотя бы 
            {{ $v.mainSection.patronymic.$params.minLength.min }} буквы
          </span>

          <input id="patronymic" type="text"
            :class="{ 'error-highlight' : $v.mainSection.patronymic.$error,
              'success-hightlight' : !$v.mainSection.patronymic.$error 
              && $v.mainSection.patronymic.$model }"
            v-model.trim="$v.mainSection.patronymic.$model" placeholder="Иванович"
          >
        </div>

        <div class="form__item">
          <label for="date-of-birth" class="decription required">Дата рождения</label>
          <span class="error-notification"
            v-show=" $v.mainSection.dateOfBirth.$dirty && !$v.mainSection.dateOfBirth.$model"
          >Обязательное поле</span>

          <input id="date-of-birth" type="date"
            required
            min="1920-01-01" max="2021-01-01"
            :class="{ 'error-highlight' : $v.mainSection.dateOfBirth.$error,
              'success-hightlight' : !$v.mainSection.dateOfBirth.$error 
              && $v.mainSection.dateOfBirth.$model }"
            autocomplete="date" 
            v-model="$v.mainSection.dateOfBirth.$model"
          >
        </div>

        <div class="form__item">
          <label for="gender" class="decription required">Пол</label>
          <div class="gender-wrap">
            <input type="radio" name="gender" id="male"
            value="male" 
            v-model="mainSection.gender"
            >
            <label for="male">М</label>
            <input type="radio" name="gender" id="female"
              value="female"
              v-model="mainSection.gender"  
            >
            <label for="female">Ж</label>
          </div>
          
        </div>

        <div class="form__item ">
          <label for="pnone-number" class="decription required">Телефон</label>
          <span class="error-notification" 
            v-if="!$v.mainSection.phoneNumber.minLength
            && $v.mainSection.phoneNumber.$dirty"> Телефон должен содержать 11 цифр
          </span>

          <span class="error-notification"
            v-show=" !$v.mainSection.phoneNumber.$model && $v.mainSection.phoneNumber.$dirty "
          >Обязательное поле</span>

          <input id="phone-number" type="tel"
            :class="{ 'error-highlight' : $v.mainSection.phoneNumber.$error,
              'success-hightlight' : !$v.mainSection.phoneNumber.$error 
              && $v.mainSection.phoneNumber.$model }"
            placeholder="7 (___) ___-__-__"
            maxlength="17"
            autocomplete="tel"
            v-model.trim="$v.mainSection.phoneNumber.$model"
            v-phone
          >
        </div>
        
        <div class="form__item">
          <label for="customer-group" class="decription required">Группа клиентов</label>
          <span class="error-notification"
            v-show=" $v.mainSection.customerGroup.$invalid
              && $v.mainSection.customerGroup.$dirty "
          >Обязательное поле</span>
          <select name="" id="customer-group"
             :class="{ 'error-highlight' : $v.mainSection.customerGroup.$invalid
              && $v.mainSection.customerGroup.$dirty,
              'success-hightlight' : !$v.mainSection.customerGroup.$error 
              && $v.mainSection.customerGroup.isEmpty}"
            multiple 
            v-model="mainSection.customerGroup"
          >
            <option 
              @click="$v.mainSection.customerGroup.$touch"
              v-for="(group, index) in customerGroups"
              :value="group.value"
              :key="index"
            > {{group.name}}</option>
          </select>
        </div>

        <div class="form__item">
          <label for="doctor" class="decription">Лечащий врач</label>
          <select name="" id="doctor" v-model="mainSection.doctor">
            <option
              v-for="(doctor, index) in doctors"
              :value="doctor.value"
              :key="index"
            > {{doctor.name}}</option>
          </select>
        </div>

        <div class="form__item">
          <input type="checkbox" id="sms" v-model="mainSection.notSendSMS">
          <label for="sms">Не отправлять СМС</label>
        </div>
      </div>
      <div class="address-section">
        <h2>Адрес</h2>
        <div class="form__item">
          <label for="index" class="decription">Индекс</label>
          <span class="error-notification"
            v-show=" $v.addressSection.index.$model 
              && !$v.addressSection.index.numeric "
          >Только цифры</span>
          <span class="error-notification"
            v-show=" $v.addressSection.index.$model 
              && !$v.addressSection.index.minLength
              && $v.addressSection.index.numeric
              || !$v.addressSection.index.maxLength
              && $v.addressSection.index.numeric
              "
          >Должно быть 6 цифр</span>
          <input id="index" type="text"
            :class="{ 'error-highlight' : $v.addressSection.index.$error,
              'success-hightlight' : !$v.addressSection.index.$error
              && $v.addressSection.index.$model }"
            v-model.trim="$v.addressSection.index.$model" placeholder="000000"
          >
        </div>

        <div class="form__item">
          <label for="country" class="decription">Страна</label>
          <span class="error-notification"
            v-show=" $v.addressSection.country.$model 
              && !$v.addressSection.country.isText "
          >Только буквы</span>
          <input id="country" type="text"
            :class="{ 'error-highlight' : $v.addressSection.country.$error,
              'success-hightlight' : !$v.addressSection.country.$error
              && $v.addressSection.country.$model }"
            v-model.trim="$v.addressSection.country.$model" placeholder="Россия"
          >
        </div>

        <div class="form__item">
          <label for="region" class="decription">Область</label>
          <span class="error-notification"
            v-show=" $v.addressSection.region.$model 
              && !$v.addressSection.region.isText "
          >Только буквы</span>
          <input id="city" type="text"
            :class="{ 'error-highlight' : $v.addressSection.region.$error,
              'success-hightlight' : !$v.addressSection.region.$error
              && $v.addressSection.region.$model }"
            v-model.trim="$v.addressSection.region.$model" placeholder="МО"
          >
        </div>

        <div class="form__item">
          <label for="city" class="decription required">Город</label>
          <span class="error-notification"
            v-show=" $v.addressSection.city.$model 
              && !$v.addressSection.city.isText "
          >Только буквы</span>
          <span class="error-notification"
            v-show=" $v.addressSection.city.$dirty 
              && !$v.addressSection.city.$model "
          >Обязательное поле</span>
          <input id="city" type="text"
            :class="{ 'error-highlight' : $v.addressSection.city.$error,
              'success-hightlight' : !$v.addressSection.city.$error 
              && !$v.addressSection.city.$invalid }"
            v-model.trim="$v.addressSection.city.$model" placeholder="Москва"
          >
        </div>

        <div class="form__item">
          <label for="street" class="decription">Улица</label>
          <input id="street" type="text"
            :class="{ 'error-highlight' : $v.addressSection.street.$error,
              'success-hightlight' : !$v.addressSection.street.$error
              && $v.addressSection.street.$model }"
            v-model.trim="$v.addressSection.street.$model" placeholder="Пушкина"
          >
        </div>
        <div class="form__item">
          <label for="house" class="decription">Дом</label>
          <input id="house" type="text"
            :class="{ 'error-highlight' : $v.addressSection.house.$error,
              'success-hightlight' : !$v.addressSection.house.$error
              && $v.addressSection.house.$model }"
            v-model.trim="$v.addressSection.house.$model" placeholder="1"
          >
        </div>
      </div>
      <div class="docs-section">
        <h2>Документ</h2>
        <div class="form__item">
          <label for="document" class="decription required">Тип документа</label>
          <select name="" id="document" v-model="docsSection.document">
            <option
              v-for="(document, index) in documents"
              :value="document.value"
              :key="index"
            > {{document.name}}</option>
          </select>
        </div>

        <div class="form__item">
          <label for="series" class="decription">Серия</label>
          <span class="error-notification"
            v-show=" $v.docsSection.series.$model 
              && !$v.docsSection.series.minLength
              || !$v.docsSection.series.maxLength
              "
          >Должно быть 4 символа</span>
          <input id="series" type="text"
            :class="{ 'error-highlight' : $v.docsSection.series.$error,
              'success-hightlight' : !$v.docsSection.series.$error
              && $v.docsSection.series.$model }"
            v-model.trim="$v.docsSection.series.$model" placeholder="0000"
          >
        </div>

        <div class="form__item">
          <label for="number" class="decription">Номер</label>
          <span class="error-notification"
            v-show=" $v.docsSection.number.$model 
              && !$v.docsSection.number.numeric "
          >Только цифры</span>
          <span class="error-notification"
            v-show=" $v.docsSection.number.$model 
              && !$v.docsSection.number.minLength
              && $v.docsSection.number.numeric
              || !$v.docsSection.number.maxLength
              && $v.docsSection.number.numeric
              "
          >Должно быть 6 цифр</span>
          <input id="number" type="text"
            :class="{ 'error-highlight' : $v.docsSection.number.$error,
              'success-hightlight' : !$v.docsSection.number.$error
              && $v.docsSection.number.$model }"
            v-model.trim="$v.docsSection.number.$model" placeholder="000000"
          >
        </div>

        <div class="form__item issued-by-wrapper">
          <label for="issued-by" class="decription ">Кем выдан</label>
          <input id="issued-by" type="text"
            :class="{ 'error-highlight' : $v.docsSection.issuedBy.$error,
              'success-hightlight' : !$v.docsSection.issuedBy.$error 
              && $v.docsSection.issuedBy.$model }"
            v-model.trim="$v.docsSection.issuedBy.$model" placeholder=""
          >
        </div>

        <div class="form__item">
          <label for="date-of-issue" class="decription required">Дата выдачи</label>
          <span class="error-notification"
            v-show=" $v.docsSection.dateOfIssue.$dirty 
              && !$v.docsSection.dateOfIssue.$model"
          >Обязательное поле</span>

          <input id="date-of-issue" type="date"
            required
            min="1920-01-01" max="2021-01-01"
            :class="{ 'error-highlight' : $v.docsSection.dateOfIssue.$error,
              'success-hightlight' : !$v.docsSection.dateOfIssue.$error 
              && $v.docsSection.dateOfIssue.$model }"
            autocomplete="date" 
            v-model="$v.docsSection.dateOfIssue.$model"
          >
        </div>

      </div>
      <button type="submit" id="create-button"
        :class="{'all-success' : !$v.$invalid }"
        v-on:click.prevent="isCompleted"
      >Создать</button>
    </form>
    </transition>

    <transition name="fade">
      <SuccessNotification
        :completed="completed"
        :surname="mainSection.surname"
        :name="mainSection.name"
        :patronymic="mainSection.patronymic"
        :phoneNum="mainSection.phoneNumber"
        :backToCreate="isCompleted"
      > 
      </SuccessNotification>  
    </transition>

  </div>
</template>

<script>
import { required, minLength, maxLength, numeric} from 'vuelidate/lib/validators'
import SuccessNotification from '@/components/SuccessNotification.vue'

export default {
  name: 'App',
  components: {
    SuccessNotification
  },
  data() {
    return {
      mainSection: {
        surname: null,
        name: null,
        patronymic: '',
        dateOfBirth: null,
        phoneNumber: null,
        gender: 'male',
        customerGroup: [],
        doctor: 'Ivanov',
        notSendSMS: false
      },
      addressSection: {
        index: null,
        country: null,
        region: null,
        city: null,
        street: null,
        house: null
      },
      docsSection: {
        document: 'pasport',
        series: null,
        number: null,
        issuedBy: null,
        dateOfIssue: null
      },
      customerGroups: [
        { name: "VIP", value: "VIP" },
        { name: "Проблемные", value: "problem" },
        { name: "ОМС", value: "OMS" }
      ],
      doctors: [
        { name: "Иванов", value: "Ivanov" },
        { name: "Захаров", value: "Zaharov" },
        { name: "Чернышева", value: "Chernyshova" }
      ],
      documents: [
        { name: "Паспорт", value: "pasport" },
        { name: "Свидетельство о рождении", value: "birth-certificate" },
        { name: "Вод. удостоверение", value: "driver's license" }
      ],
      completed: false
    }
  },
  validations: {
    mainSection: {
      surname: { required, minLength: minLength(2) },
      name: { required, minLength: minLength(2) },
      patronymic: { minLength: minLength(2)},
      dateOfBirth: { required },
      phoneNumber: { required, minLength: minLength(17) },
      customerGroup: {
        required,
        isEmpty: value => value.length !== 0
      }
    },
     addressSection: {
        index: { numeric, minLength: minLength(6), maxLength: maxLength(6) },
        country: { isText: value => !/[^a-zа-яё ]/iug.test(value) },
        region: { isText: value => !/[^a-zа-яё ]/iug.test(value) },
        city: { 
          required,
          isText: value => !/[^a-zа-яё ]/iug.test(value)
        },
        street: {},
        house: {}
      },
      docsSection: {
        document: { required },
        series: { minLength: minLength(4), maxLength: maxLength(4) },
        number: { numeric, minLength: minLength(6), maxLength: maxLength(6) },
        issuedBy: {},
        dateOfIssue: { required }
      },
    
  },
  methods: {
    isCompleted() {
      this.$v.$touch()
      if (!this.$v.$invalid ) {
        this.completed = !this.completed
      }
      return
    }
  },
  computed: {
    notFormatPhoneNumber() {
      if(this.mainSection.phoneNumber) {
        let tempPhone = this.mainSection.phoneNumber.match(/\d/g).join('')
        return tempPhone
      }
      return ''
    }

  },
  directives: {
    phone: {
      bind(el) {  
        el.oninput = function(e) {
          if (!e.isTrusted) {
            return;
      }

      const x = this.value.replace(/\D/g, '').match(/(\d{0,1})(\d{0,3})(\d{0,3})(\d{0,2})(\d{0,2})/);
      if (x[1]) x[1] = "7"
      this.value = !x[2] ?  x[1] : x[1] + ' (' + x[2] + ') ' + x[3] + (x[3] ? '-' + x[4] : '') 
                  + (x[4] ? '-' + x[5] : '');
      el.dispatchEvent(new Event('input'));
    } 
  },
    }
  }
}
</script>

<style lang="sass">
@import url('https://fonts.googleapis.com/css2?family=Play:wght@400;700&display=swap')

*
  font-family: 'Play', sans-serif
  box-sizing: border-box

form
  display: grid
  padding: 5px
  gap: 10px
  grid-template-areas: "main main" "address docs" 
  grid-template-columns: 1fr 1fr
  grid-template-rows: 1fr 1fr 40px
  box-shadow: 0px 1px 3px 0px #ccc

  button 
    padding: 0
    border: none
    font: inherit
    color: inherit
    background-color: transparent

  #create-button
    background-color: #ddd
    color: #000
    justify-self: end
    width: 200px
    grid-column-start: 1
    grid-column-end: 3
    margin-right: 40px
    font-size: 1.2rem
    outline: none
    cursor: pointer

    &:hover
      transform: translateY(1px)
      filter: saturate(150%)
       
  
  .all-success
    background-color: #52B152 !important
    color: #fff
h2
  grid-column-start: 1
  grid-column-end: 4
  margin-bottom: 0
  padding-left: 20px
  max-width: 90%
  border-bottom: 1px solid #ccc 

label.decription
  display: block
  position: relative
  font-size: .9rem

.required
  &::after
    content: '*'
    position: absolute
    left: -6px
    top: -6px

.main-section
  grid-area: main
  padding: 0px 10px 10px
  border: 1px solid #ccc
  border-radius: 5px
  display: grid
  grid-row-gap: 30px
  grid-template-columns: repeat(3, 1fr)

.address-section
  grid-area: address
  padding: 0px 10px 10px
  margin-bottom: 10px
  border: 1px solid #ccc
  border-radius: 5px
  display: grid
  grid-row-gap: 30px
  grid-template-columns: 1fr 1fr

  h2
    grid-column-start: 1
    grid-column-end: 3


.docs-section
  grid-area: docs
  padding: 0px 10px 10px
  margin-bottom: 10px
  border: 1px solid #ccc
  border-radius: 5px
  display: grid
  grid-row-gap: 30px
  grid-template-columns: 1fr 1fr 1fr

  .issued-by-wrapper
    grid-column-start: 1
    grid-column-end: 3

    input
      width: 100%

.form__item
  disply: flex
  justify-content: space-between
  flex-direction: column
  padding: 10px
  padding-bottom: 0px
  position: relative
  border-left: 1px solid #ddd 

  span
    position: absolute
    top: -5px
    font-size: .9rem
    color: rgba(255, 0, 0, .9) 
  
  input, select
    font-size: 1rem
    padding: 10px
    border: none
    border-bottom: 1px solid #ccc
    transition: border-width .3s ease

    &:focus
      outline: none
      border: 1px solid #ccc
      border-radius: 5px

  .error-highlight
    border-bottom: 1px solid rgba(255, 0, 0, .9)

  .success-hightlight
    border-bottom: 1px solid rgba(0, 255, 0, .9)

#customer-group
  overflow: hidden
  height: 80px
 
.gender-wrap
  max-width: 200px
  padding: 10px
  border-bottom: 1px solid #ccc    


.fade-enter-active, .fade-leave-active
  transition: opacity .7s

.fade-enter, .fade-leave-to
  opacity: 0



@media screen and (max-width: 1240px)
  form
    display: flex
    flex-direction: column

    #create-button
      padding: 10px
      align-self: flex-end


@media screen and (max-width: 750px)
  .main-section
    display: grid
    grid-row-gap: 10px
    grid-template-columns: 1fr 1fr
    grid-template-rows: repeat(6, 1fr)
    grid-auto-flow: column
    h2
      grid-column-start: 1
      grid-column-end: 3

  .docs-section
   grid-template-columns: 1fr 1fr
   grid-template-rows: repeat(4, 1fr)

  .issued-by-wrapper
    grid-column-start: 1
    grid-column-end: 2
    order: 1
    
  h2
    grid-column-start: 1
    grid-column-end: 3
    order: 0

@media screen and (max-width: 520px)
  .main-section, .address-section, .docs-section
    display: flex
    flex-direction: column
</style>
