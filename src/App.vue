<script>
import Cards from './Cards.vue'
import { store } from './store'
import axios from 'axios'

export default {
  data() {
    return {
      store
    }
  },
  methods: {
    getImagePath: function (imgPath) {
      return new URL(imgPath, import.meta.url).href;
    },
    writeStore(link) {
      this.store.loading = true;
      axios.get(link).then(element => {
        const result = element.data;
        this.store.cardsArray = result;
        this.store.loading = false;
      });
    }
  },
  mounted() {
    this.writeStore(this.store.urlAPI);
  },
  computed: {
    dimension() {
      return this.store.cardsArray.length;
    }
  }
}
</script>

<template>
  <header class="flex jc-center ai-center">
    <div class="std-width flex ai-center">
      <img src="https://upload.wikimedia.org/wikipedia/commons/1/11/Yu-Gi-Oh%21_%28Logo%29.jpg">
      <h1>Yu-Gi-Oh Api</h1>
    </div>
  </header>
  <main class="p-2 flex jc-center ai-center bg-orange">
    <div class="std-width flex ai-center bg-white">
      <div class="p-2 flex ai-center fd-columns">
        <!-- TOP -->
        <div class="w-100 p-1 flex ai-center bg-grey">
          <p>
            Sono state trovate {{ this.dimension }} carte
          </p>
        </div>
        <!-- CARDS -->
        <div class="w-100 flex fw-wrap">
          <Cards />
        </div>
      </div>
    </div>
  </main>
</template>

<style lang="scss" scoped>
@use './style/specific/utility.scss' as *;
@use './style/specific/style.scss' as *;
</style>
