<script>
import { store } from './store'
import axios from 'axios'
import CardTest from './components/CardTest.vue'

export default {
  data() {
    return {
      store,
      filteredArray: [],
      selected: "All"
    }
  },
  methods: {
    writeStore(link) {
      this.store.loading = true;
      axios.get(link).then(element => {
        const result = element.data;
        this.store.cardsArray = result.data;
        this.store.loading = false;
        this.store.cardsArray.forEach(element => {
          if (!this.filteredArray.includes(element.race)) {
            this.filteredArray.push(element.race);
          }
        });
      });
    }
  },
  mounted() {
    this.writeStore(this.store.urlAPI);
  },
  computed: {
    takeData() {
      return this.store.cardsArray;
    },
    dimension() {
      return this.store.cardsArray.length;
    }
  }
}
</script>

<template>
  <!--***  -->
  <CardTest />
  <!-- *** -->
  <div v-if="store.loading == false">
    <header class="flex jc-center ai-center">
      <div class="std-width flex ai-center">
        <img src="https://upload.wikimedia.org/wikipedia/commons/1/11/Yu-Gi-Oh%21_%28Logo%29.jpg">
        <h1>Yu-Gi-Oh Api</h1>
      </div>
    </header>
    <main class="p-2 flex jc-center ai-center fd-columns bg-orange">
      <div class="std-width pb-2">
        <select v-model="selected">
          <option>All</option>
          <option v-for="filter in filteredArray">{{ filter }}</option>
        </select>
      </div>
      <div class="std-width flex ai-center bg-white">
        <div class="p-2 flex ai-center fd-columns">
          <!-- TOP -->
          <div class="w-100 p-1 flex ai-center bg-grey">
            <p class="c-white">
              There are currently {{ dimension }} cards
            </p>
          </div>
          <!-- CARDS -->
          <div class="w-100 flex fw-wrap jc-between gap-1">
            <div v-for="gameCard in takeData" class="bg-orange w-168px">
              <div>
                <img :src="gameCard.card_images[0].image_url_small">
              </div>
              <div class="p-1 flex jc-center ai-center fd-columns gap-1 w-wrap ta-center">
                <h3 class="c-white upperCase">{{ gameCard.name }}</h3>
                <p>{{ gameCard.race }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<style lang="scss" scoped>
@use './style/specific/utility.scss' as *;
@use './style/specific/style.scss' as *;
</style>
