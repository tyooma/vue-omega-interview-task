<template>
  <div id="app" class="app">
    <img
      src="./images/logo.svg"
      alt="logo"
      class="app__logo"
    />
    <Form />
  </div>
</template>

<script>
import Form from './components/Form';

export default {
  name: 'App',
  components: {
    Form,
  }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Manrope:wght@500;700&display=swap');

* {
  box-sizing: border-box;
}

body {
  background-color: #f1f5fa;
}

.app {
  font-family: Manrope, sans-serif;
  font-style: normal;
  font-weight: 500;

  &__logo {
    margin: 40px 0 0 40px;

    cursor: pointer;
  }
}
</style>
