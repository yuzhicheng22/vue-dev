<script setup lang="ts">
import { ref, onMounted, reactive, computed } from 'vue'
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
</script>

<template>
  <button @click="increment">count is: {{ count }}</button>
  <button @click="reset">reset</button>

  <div id="bookshelf">
    <input type="text" v-model="newBookName" />
    <span>{{ showBookShelf }}</span>
    <button @click="addBook()">添加书本</button>
    <button @click="removeBook()">移除书本</button>
    <div v-for="book in bookshelf.books">
      {{ book.name }}
    </div>
  </div>
</template>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
</style>
