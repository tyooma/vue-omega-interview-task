<template>
  <div>
    <form v-if="!completed" class="form" @submit.prevent="onSumbit">
      <p class="form__title">Welcome to OMG</p>
      <div class="form__items-container">
        <div class="form__item">
          <input
            type="text"
            id="firstName"
            placeholder="Alex"
            class="form__input"
            v-model.trim="$v.firstName.$model"
            :class="{form__invalid: ($v.firstName.$dirty && !$v.firstName.required)
              || ($v.firstName.$dirty && !$v.firstName.minLength) || ($v.firstName.$dirty && !$v.firstName.maxLength)}"
          >
          <label for="firstName" class="form__label">First Name</label>
        </div>
        <div class="form__item">
          <input
            type="text"
            id="secondName"
            placeholder="Green"
            class="form__input"
            v-model.trim="$v.secondName.$model"
            :class="{form__invalid: ($v.secondName.$dirty && !$v.secondName.required)
              || ($v.secondName.$dirty && !$v.secondName.minLength) || ($v.secondName.$dirty && !$v.secondName.maxLength)}"
          >
          <label for="secondName" class="form__label">Second Name</label>
        </div>
      </div>
      <div class="form__item">
        <input 
          type="text"
          id="phone"
          placeholder="+44 (xxxx) xxxxxx"
          class="form__input--block"
          v-model.trim="phone"
          :class="{form__invalid: ($v.phone.$dirty && !$v.phone.required) || ($v.phone.$dirty && !$v.phone.isUnique)}"
        >
        <label for="phone" class="form__label">Phone</label>
      </div>
      
      <div class="form__item">
        <input 
          type="text"
          id="email"
          placeholder="example@mail.com"
          class="form__input--block"
          v-model.trim="$v.email.$model"
          :class="{form__invalid: ($v.email.$dirty && !$v.email.required) || ($v.email.$dirty && !$v.email.email)}"
        >
        <label for="email" class="form__label">Email</label>
      </div>
      
      <button class="form__button" v-bind:disabled="isDisabled">
        Create Account
      </button>
    </form>
    <div v-if="completed" class="success">
      <img src="../images/verified.svg" alt="Success logo">
      <p class="success__text">Thank you!</p>
    </div>
  </div>
</template>

<script>
import {
  required,
  minLength,
  maxLength,
  email,
} from 'vuelidate/lib/validators';

export default {
  name: 'Form',
  data () {
    return {
      firstName: '',
      secondName: '',
      phone: '',
      email: '',
      completed: false
    }
  },
  validations: {
    firstName: {
      required,
      minLength: minLength(3),
      maxLength: maxLength(10),
    },
    secondName: {
      required,
      minLength: minLength(3),
      maxLength: maxLength(10),
    },
    phone: {
      required,
      isUnique(value) {
        if (value === '') return true

        const numberRegex = /\+44\s\(\d{4}\)\s\d{6}/;

        return numberRegex.test(value);
      }
    },
    email: {
      required,
      email,
    }
  },
  methods: {
    onSumbit() {
      this.completed = true;
    }
  },
  computed: {
    isDisabled: function() {
      return this.$v.$invalid ? true : false;
    }
  }
}
</script>

<style lang="scss">
  @mixin input($width) {
    padding: 27px 0 12px 28px;
    margin-bottom: 16px;

    width: $width;

    font-size: 14px;
    line-height: 19px;

    letter-spacing: -0.01em;

    background-color: #fff;
    box-shadow: 0px 1px 1px rgba(0, 66, 142, 0.25);
    border: none;
    border-radius: 10px;

    &::placeholder {
      opacity: 0;
    }

    &:focus {
      background-size: 100% 100%, 100% 100%;
      box-shadow: none;
      outline: none;

      transition-duration: 0.3s;
    }


    &:focus+label,
    &:not(:placeholder-shown)+label {
      top: 11px;

      font-size: 10px;
      line-height: 14px;
      letter-spacing: -0.01em;
    }
  }

  .form {
    margin: 150px auto 0;

    width: 556px;
    height: 335px;

    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-between;

    &__title {
      margin-bottom: 28px;

      font-size: 16px;
      line-height: 26px;

      color: #333333
    }

    &__items-container {
      width: 100%;

      display: flex;
      justify-content: space-between;
    }

    &__item {
      position: relative;
    }

    &__input {
      @include input(270px);
    }

    &__input--block {
      @include input(556px);
    }

    &__label {
      position: absolute;
      top: 20px;
      left: 28px;

      font-size: 14px;
      line-height: 19px;
      letter-spacing: -0.01em;

      color: #7188a3;

      transition: top 0.5s;
    }

    &__invalid {
      border: 1px solid #ff1a1a;
      box-shadow: 0 1px 1px rgba(0, 66, 142, 0.25);
      border-radius: 10px;
    }

    &__button {
      padding: 13px 33px;
      margin: 8px 0 0 auto;

      font-size: 18px;
      line-height: 25px;
      text-align: center;
      letter-spacing: -0.02em;

      color: #fff;
      background-color: #01a3ff;
      border: none;
      border-radius: 60px;
      outline: none;

      cursor: pointer;

      transition: background-color 0.3s;

      &:hover {
        background-color: #0090e4;
      }

      &:disabled {
        background: rgba(1, 163, 255, 0.25);
      }
    }
  }

  .success {
    margin: 250px auto 0;

    width: 97px;
    height: 93px;

    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;

    font-weight: bold;
    font-size: 18px;
    line-height: 25px;
    letter-spacing: -0.02em;

    color: #000;

    &__text {
      margin: 16px 0 0;
    }
  }
</style>
