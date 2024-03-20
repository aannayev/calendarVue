<template>

  <div class="language-buttons">
    <button @click="changeLanguage('en')">English</button>
    <button @click="changeLanguage('ru')">Русский</button>
  </div>
  <CalendarVue
    :initial-date="'2024-03-20'"
    :language="currentLanguage"
    @date-selected="handleDateSelected"
  />
</template>

<script>
import CalendarVue from './components/CalendarVue.vue'

export default {
  name: 'App',
  components: {
    CalendarVue
  },
  data() {
    return {
      currentLanguage: 'en'
    };
  },
  methods: {
    handleDateSelected(selectedDate) {
      console.log('Selected Date:', selectedDate);

    },
    changeLanguage(language) {
      this.currentLanguage = language;
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>./components/CalendarVue.vue
