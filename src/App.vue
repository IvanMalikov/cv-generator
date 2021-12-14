<template>
  <div class="container column">
    <form class="card card-w30" @submit.prevent>
      <div class="form-control">
        <label for="type">Тип блока</label>
        <select id="type" v-model="selectedOption">
          <option value="title">Заголовок</option>
          <option value="subtitle">Подзаголовок</option>
          <option value="avatar">Аватар</option>
          <option value="text">Текст</option>
        </select>
      </div>

      <div class="form-control">
        <label for="value">Значение</label>
        <textarea id="value" rows="3" v-model="textareaValue"></textarea>
      </div>

      <button class="btn primary" @click="formSubmit" :disabled="disableBtn">Добавить</button>
    </form>


    <div class="card card-w70">
      <h3  v-if="!isComponent">Добавьте первый блок, чтобы увидеть результат</h3>
      <div v-for="item in componentContent" :key="item">
        <component :is="'app-' + item.name" :value="item.value"></component>
      </div>
    </div>
    
  </div>
  <div class="container">
    <p v-if="comments.length === 0">
      <button class="btn primary" @click="loadComments">Загрузить комментарии</button>
    </p>
    <div class="card" v-else>
      <h2>Комментарии</h2>
      <ul class="list">
        <li class="list-item" v-for="comment in comments" :key="comment">
          <div>
            <h3>{{ comment.name }}</h3>
            <p>{{ comment.text }}</p>
          </div>
        </li>
      </ul>
    </div>
  </div>
  <app-loader v-if="loading"></app-loader>
</template>

<script>
  /* eslint-disable */
  import AppLoader from './components/AppLoader.vue'
  import AppAvatar from './components/AppAvatar.vue'
  import AppTitle from './components/AppTitle.vue'
  import AppSubtitle from './components/AppSubtitle.vue'
  import AppText from './components/AppText.vue'
  
  import axios from 'axios'
  export default {
    data() {
      return {
        isComponent: false,
        componentContent: [],
        selectedOption: 'title',
        textareaValue: '',
        comments: [],
        loading: false,
      }
    },

    methods: {
      formSubmit() {
        this.isComponent = true
        this.componentContent.push({
          name: this.selectedOption,
          value: this.textareaValue
        })
        this.textareaValue = ''
        this.selectedOption = 'title'
      },

      async loadComments() {
        this.loading = true
        const {data} = await axios.get('https://jsonplaceholder.typicode.com/comments?_limit=5')
        this.comments = Object.keys(data).map(key => {
          return {
            name: data[key].email,
            text: data[key].body
          }
        })
        this.loading = false
      }
    },

    computed: {
      disableBtn() {
        return this.textareaValue.length < 4
      }
    },

    components: {
      AppLoader,
      AppAvatar,
      AppTitle,
      AppSubtitle,
      AppText
    }
  }
</script>

<style>
  
</style>
