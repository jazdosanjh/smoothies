<template>
  <div class="index container">
    <div class="card" v-for="smoothie in smoothies" :key="smoothie.id">
      <div class="card-content">
        <i class="material-icons delete" @click="deleteSmoothie(smoothie.id)">delete</i>
        <h2 class="indigo-text">{{ smoothie.title }}</h2>
        <ul class="ingredients">
          <li v-for="(ing, index) in smoothie.ingredients" :key="index">
            <span class="chip">{{ ing }}</span>
          </li>
        </ul>
      </div>
      <span class="btn-floating btn-large halfway-fab pink">
        <router-link :to="{ name: 'EditSmoothie', params: {smoothie_slug: smoothie.slug} }">
          <i class="material-icons edit">edit</i>
        </router-link>
      </span>
    </div>
    </div>
</template>

<script>
import db from '@/firebase/init'
export default {
  name: 'Index',
  data () {
    return {
      smoothies: [] 
    }
  },
  methods: {
    deleteSmoothie(id) {
      // Delete doc from firestore
      db.collection('smoothies').doc(id).delete()
      .then(() => {
        this.smoothies = this.smoothies.filter(smoothie => {
          return smoothie.id != id
        })
      })
    }
  },
  created() {
    // Fetch data from the firestore
    db.collection('smoothies').get()
    .then(snapshot => {
      snapshot.forEach(doc => {
        let smoothie = doc.data()
        smoothie.id = doc.id
        this.smoothies.push(smoothie)
      })
    })
  }
}
</script>

<style>
.index {
  margin-top: 60px;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 30px;
}

.index h2 {
  margin-top: 0;
  font-size: 1.8em;
  text-align: center;
}

.index .ingredients {
  margin: 30px auto;
}

.index .ingredients li {
  display: inline-block;
}

.index .delete {
  position: absolute;
  top: 4px;
  right: 4px;
  cursor: pointer;
  color: #AAA;
  font-size: 1.4em;
}
</style>
