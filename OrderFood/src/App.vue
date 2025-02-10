<script setup>
import { Menu, X } from 'lucide-vue-next';
import { ref } from 'vue'

const showDiv = ref(false)
const showScreenOrders = ref(false)
const buttonScreenOrders = ref(Menu)
const userName = ref('')
const food = ref('hamburguer')
const drink = ref('soda')
// Lists
const usersList = ref([]) 
const foodsList = ref([])
const drinksList = ref([])

const foods = ref({
  food1: "Hamburguer",
  food2: "Pizza",
  food3: "None",
})

const drinks = ref({
  drink1: "Soda",
  drink2: "Juice",
  drink3: "None",
})

function toggleshowDiv() {
  showDiv.value = !showDiv.value
}

function pushlistOrder() {
  showDiv.value = false
  if (userName.value.length > 8, userName.value.length < 1) {
    alert("Name Invalid!")
  } else if (userName.value.trim()) {
    usersList.value.push(userName.value)
    userName.value = ''
    foodsList.value.push(food)
    drinksList.value.push(drink)
  }
}

function endOrder(index) {
    usersList.value.splice(index, 1)
    foodsList.value.splice(index, 1)
    drinksList.value.splice(index, 1)
}

function toggleshowScreenOrders() {
  showDiv.value = false
  showScreenOrders.value = !showScreenOrders.value
  buttonScreenOrders.value = showScreenOrders.value ? X : Menu
}
</script>

<template>
  <div id="app" class="flex flex-row min-h-screen justify-center items-center">
    <header>
      <button 
        @click="toggleshowScreenOrders" 
        class="absolute top-12 right-16 p-2 rounded-full hover:rotate-180 hover:scale-115 text-green-400 cursor-pointer duration-300">
        <component :is="buttonScreenOrders" class="w-7 h-7"></component></button>
    </header>
    <main class="animate-fade-up">
      <!-- Screen Order -->
      <div v-if="showScreenOrders">
        <!-- Name of User -->
        <div class="flex gap-13 text-2xl relative bottom-73">
          <ul class="font-bold"> 
          <label class="text-green-400">Name</label>
          <li v-for="(user, userList) in usersList" :key="userList" class="text-gray-800 text-xl">{{ user }}</li>
          </ul>
            <!-- Food of User -->
            <ul class="text-green-400 font-bold"> 
              <label class="text-green-400">Food</label>
              <li v-for="(food, foodList) in foodsList" :key="foodList" class="text-gray-800 text-xl">{{ food }}</li>
            </ul>
            <!-- Drink of User -->
            <ul class="text-green-400 font-bold">
              <label class="text-green-400">Drink</label>
              <li v-for="(drink, drinkList) in drinksList" :key="drinkList" class="text-gray-800 text-xl">{{ drink }}</li>
            </ul>
            <div class="flex flex-col justify-center items-center relative">
              <button 
                v-for="(user, index) in usersList" :key="index" class="bg-green-400 rounded text-lg w-20 cursor-pointer text-white relative top-3 gap-2" @click="endOrder(index)">Done
              </button>
            </div>
        </div>
      </div>
      <!-- Screen of do Order -->
      <div v-else>
        <div class="flex flex-row justify-center items-center">
          <Transition enter-active-class="animate-jump-in" leave-active-class="animate-jump-out">
              <div v-if="showDiv" class="group animate-jump-in bg-white shadow-2xl rounded w-90 h-90 justify-center items-center flex flex-row absolute">
                <form @submit.prevent="pushlistOrder" class="flex flex-col gap-1">
                  <!-- User -->
                  <label for="name" class="text-neutral-900 font-bold">Name</label>
                  <input type="text" class="rounded border-2 border-green-400 font-bold text-neutral-900 focus:outline-none" v-model="userName">
                  <!-- Food -->
                  <label for="food" class="text-neutral-900 font-bold">Food</label>
                  <select name="food" class="focus:outline-none bg-green-400 text-white rounded" v-model="food">
                    <option value="hamburguer">{{ foods.food1 }}</option>
                    <option value="pizza">{{ foods.food2 }}</option>
                    <option value="none">{{ foods.food3 }}</option>
                  </select>

                  <!-- Drink -->
                  <label for="food" class="text-neutral-900 font-bold">Drink</label>
                  <select name="drinks" class="focus:outline-none bg-green-400 text-white rounded" v-model="drink">
                    <option value="soda">{{ drinks.drink1 }}</option>
                    <option value="juice">{{ drinks.drink2 }}</option>
                    <option value="none">{{ drinks.drink3 }}</option>
                  </select>
                <button class="rounded group-animate-jump-out bg-green-400 text-white p-2 w-60 cursor-pointer top-10 relative hover:bg-green-500 flex justify-center" @click="pushlistUsers">Complete the Order</button>
              </form>
            </div>
          </Transition>
        </div>
        <button @click="toggleshowDiv" class="p-2 w-50 cursor-pointer bg-green-400 text-white rounded hover:bg-green-500">Order</button>
      </div>
    </main>
  </div>
</template>

<style scoped>
</style>