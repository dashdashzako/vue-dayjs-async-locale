<script setup>
import dayjs from 'dayjs';
import { ref, watchEffect } from 'vue';

const langs = ['fr', 'en', 'ja']
const currentLang = ref('fr')
const currentDate = ref('')

const loadLocale = async (lang) => {
  switch (lang) {
    case 'en':
      // `import` statement has to be static so chunks can be generated.
      // This function should be automatically generated parsing dayjs/locale folder.
      await import('dayjs/locale/en.js')
      break;
    case 'fr':
      await import('dayjs/locale/fr.js')
      break;
    case 'ja':
      await import('dayjs/locale/ja.js')
      break;
    default:
      throw Error('Unknown locale')
  }
}

watchEffect(async () => {
  await loadLocale(currentLang.value)
  currentDate.value = dayjs().locale(currentLang.value).format('dddd')
})
</script>

<template>
  <main>
    <select v-model="currentLang">
      <option v-for="lang in langs" :value="lang" :key="lang">{{ lang }}</option>
    </select>
    <br>

    Current lang: {{ currentLang }}<br>
    Current date: {{ currentDate }}
  </main>
</template>
