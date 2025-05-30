<script setup lang="ts">
import { ref, onMounted, reactive, computed } from 'vue'
import BtnCommon from '../common/BtnCommon.vue'
import DocumentationIcon from '../icons/IconDocumentation.vue'

//e.g1：ref
const count = ref(0)
const defaultCount = ref(-1)
function reset() {
  count.value = defaultCount.value
}

function increment() {
  count.value++
}

onMounted(() => {
  console.log(`The initial count is ${count.value}.`)
})

//e.g2：reactive
const raw = {
  name: 'Vue',
}
const proxy = reactive(raw)
console.log(reactive(proxy) === proxy) //如果已经是代理，直接返回代理本身，所以为true
console.log(reactive(raw) === raw) //返回的是代理，和原始值不同，所以为false
console.log(reactive(raw) === proxy) //在同一个对象上调用，返回相同的代理，所以为true

//e.g3：ref解包
const refbox = reactive([ref('Vue 3 Guide')])
// 这里需要 .value
console.log(refbox[0])

//e.g4：计算属性
const bookshelf = reactive({
  name: 'Math',
  books: [
    {
      name: 'Vue 3 Guide',
    },
  ],
})

function addBook() {
  bookshelf.books.push({
    name: newBookName.value,
  })
  console.log(bookshelf.books)
}

function removeBook() {
  bookshelf.books.pop()
  console.log(bookshelf.books)
}

const newBookName = ref('')
const showBookShelf = computed(() => {
  return bookshelf.books.length > 1 ? 'books' : 'book'
})
// showBookShelf.value = false 错误写法，因为计算属性是只读的
// 可以通过同时提供 getter 和 setter 来创建一个计算属性
const canWrite = ref(0)
const canWriteComputed = computed({
  get: () => {
    return canWrite.value
  },
  set: (newValue) => {
    canWrite.value = newValue
  },
})
canWriteComputed.value = 1

const isActive = computed(() => {
  return canWrite.value > 0
})

//e.g5：input
const message = ref('')
const messageArea = ref('')
const picked = ref('')
const checkedNames = ref([])
const selected = ref('')
const options = ref([
  { text: 'One', value: 'A' },
  { text: 'Two', value: 'B' },
  { text: 'Three', value: 'C' },
])
</script>

<template>
  <button @click="increment">count is: {{ count }}</button>
  <button @click="reset">reset</button>
  <BtnCommon>
    <template #icon>
      <DocumentationIcon />
    </template>
    <template #title> Select </template>
  </BtnCommon>
  <div id="bookshelf">
    <input type="text" v-model="newBookName" />
    <span>{{ bookshelf.books.length }}{{ showBookShelf }}</span>
    <button @click="addBook()">添加书本</button>
    <button @click="removeBook()">移除书本</button>
    <div v-for="book in bookshelf.books">
      {{ book.name }}
    </div>
  </div>

  <div :class="{ active: isActive }">Shoul I show?</div>

  <div class="input">
    <input type="text" v-model="message" placeholder="add single line" />
    <p>Message is {{ message }}</p>
    <textarea v-model="messageArea" placeholder="add multiple lines"></textarea>
    <p style="white-space: pre-line">MessageArea is {{ messageArea }}</p>

    <p>Select is {{ picked }}</p>
    <input type="radio" id="one" value="One" v-model="picked" />
    <label for="one">One</label>
    <input type="radio" id="two" value="Two" v-model="picked" />
    <label for="two">Two</label>

    <div>
      Checked names:
      <li v-for="name in checkedNames">{{ name }}</li>
    </div>
    <input type="checkbox" id="jack" value="Jack" v-model="checkedNames" />
    <label for="jack">Jack</label>
    <input type="checkbox" id="john" value="John" v-model="checkedNames" />
    <label for="john">John</label>
    <input type="checkbox" id="mike" value="Mike" v-model="checkedNames" />
    <label for="mike">Mike</label>
    <div>Selected: {{ selected }}</div>
    <select v-model="selected" multiple>
      <option disabled value="">Please select one</option>
      <option v-for="option in options" :value="option.value">
        {{ option.text }}
      </option>
    </select>
  </div>
</template>

<style>
button {
  text-decoration: none;
  color: hsla(160, 100%, 37%, 1);
  transition: 0.4s;
  padding: 3px;
}
</style>
