<template>
  <div class="hello">
    <SndComp v-if="toggle" />
    <h1 v-else>{{ msg }}, vous avez en moyenne {{ age }} ans</h1>
    <button @click="changeToggle">Toggle</button>
    <ul>
      <li v-for="friend in friends" :key="friend.name">{{ friend.name }} à des vêtements {{ friend.color }}</li>
    </ul>
    Incrementation : {{ age }}
    <div>
      <button @click="inc">+</button>
      <button @click="dec">-</button>
    </div>
    <label>
      V-model :
      <input type="number" v-model="age">
    </label>
    <h3>List de courses</h3>
    <form @submit.prevent="addToElements">
      <label>
        Ajouter un élément:
        <input type="text" v-model="newElement">
      </label>
      <input type="submit">
    </form>
    <ul>
      <li class="listElem" v-for="(elem, index) in elements" :key="elem.name" @click="deleteElement(index)">{{ elem.name }}</li>
    </ul>
    <label>
      Select a dev:
      <select v-model="selectedDev" @change="getEvanUser">
        <option v-for="dev in devs" :key="dev" :value="dev">{{ dev }}</option>
      </select>
    </label>
    <h3>{{ jeremy.login }}</h3>
    <p>Followers: {{ jeremy.followers }}</p>
    <img :src="jeremy.avatar_url" alt="">
  </div>
</template>

<script>
import {ref} from "@vue/reactivity";
import SndComp from "@/components/SndComp";
import {onMounted} from "@vue/runtime-core";
import axios from "axios";

export default {
  name: 'HelloWorld',
  components: { SndComp },
  props: {
    msg: String
  },
  setup() {
    const age = ref(30)
    const toggle = ref(false)
    const newElement = ref('')
    const jeremy = ref('')
    const friends = [
      {name: "Riri", color: "rouge"},
      {name: "Fifi", color: "bleu"},
      {name: "Loulou", color: "vert"},
    ]
    const devs = ["Jdevvv", "mathieucollet", "Ayce45"]
    const selectedDev = ref("Jdevvv")
    const elements = ref([])

    const getEvanUser = async () => {
      console.log(selectedDev.value)
    await axios.get(`https://api.github.com/users/${selectedDev.value}`)
      .then((res) => {
        jeremy.value = res.data
      })
    }
    onMounted(getEvanUser)


    const addToElements = () => {
      elements.value.push({name: newElement.value})
      newElement.value = ''
    }
    const deleteElement = (index) => {
      elements.value.splice(index, 1)
    }
    const changeToggle = () => {
      toggle.value = !toggle.value
    }
    const inc = () => {
      age.value++
    }
    const dec = () => {
      age.value--
    }
    return { age, toggle, friends, elements, newElement, jeremy, devs, selectedDev, changeToggle, inc, dec, addToElements, deleteElement, getEvanUser }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  margin: 10px 10px;
}
a {
  color: #42b983;
}
input {
  margin: 20px 0 0;
}
.listElem {
  cursor: pointer;
}
button {
  border: none;
  background-color: #fff;
  box-shadow: rgba(0, 0, 0, 0.16) 0px 1px 4px;
  padding: 4px 24px;
  font-weight: bold;
  transition: box-shadow .2s;
}
button:hover {
  box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
  cursor: pointer;
}
</style>
