<script setup>
import { ref, computed, watch } from 'vue'
import moment from 'moment'
import Posts from './components/Posts.vue'

const posts = ref([
  {
    id: 1,
    title: 'Post 1',
    content:
      'This is a wider card with supporting text below as a natural lead-in to additional content. This content is a little bit longer.',
    likes: 0,
    comments: [],
    date: '2024-05-24 11:00:00',
    commentFormData: '',
    showCommentSection: false
  },
  {
    id: 2,
    title: 'Post 2',
    content:
      'This is a wider card with supporting text below as a natural lead-in to additional content. This content is a little bit longer.',
    likes: 0,
    comments: [],
    date: '2024-05-24 11:00:00',
    commentFormData: '',
    showCommentSection: false
  },
  {
    id: 3,
    title: 'Post 3',
    content:
      'This is a wider card with supporting text below as a natural lead-in to additional content. This content is a little bit longer.',
    likes: 0,
    comments: [],
    date: '2024-05-24 11:00:00',
    commentFormData: '',
    showCommentSection: false
  }
])

const firstName = 'John'
const lastName = 'Doe'

const userName = computed(() => {
  return firstName + ' ' + lastName
})
// const userName = ref('Asif Mallik');

function increaseLikeCount(index) {
  posts.value[index].likes++
}

function deletePost(index) {
  posts.value.splice(index, 1)
}

const formData = ref({
  title: '',
  content: ''
})

// function postCreate(event) {
//   event.preventDefault();
function postCreate() {
  posts.value.push({
    id: posts.value.length + 1,
    title: formData.value.title,
    content: formData.value.content,
    likes: 0,
    comments: [],
    date: moment().format('YYYY-MM-DD HH:mm:ss')
  })

  formData.value.title = ''
  formData.value.content = ''
}

function addComment(index) {
  posts.value[index].comments.push({
    id: posts.value[index].comments.length + 1,
    userName: userName.value,
    content: posts.value[index].commentFormData,
    date: moment().format('YYYY-MM-DD HH:mm:ss')
  })

  posts.value[index].commentFormData = ''
  posts.value[index].showCommentSection = true
}

function deleteComment(postIndex, commentIndex) {
  posts.value[postIndex].comments.splice(commentIndex, 1)
}

watch(
  () => posts.value.length,
  (newValue, oldValue) => {
    if (newValue > oldValue) {
      alert('A new post has been created!')
    }
  },
  { immediate: true }
)

watch(
  () => posts.value,
  () => {
    posts.value.forEach((post) => {
      if (post.likes === 10 && !post.likeConfirmation) {
        post.likeConfirmation = true
        alert('A post has reached 10 likes!')
      }
    })
  },
  { deep: true }
)
</script>

<template>
  <nav class="navbar navbar-expand-lg navbar-dark bg-dark sticky-top">
    <div class="container">
      <a class="navbar-brand" href="#">BAT Blog</a>
      <button
        class="navbar-toggler"
        type="button"
        data-bs-toggle="collapse"
        data-bs-target="#navbarSupportedContent"
        aria-controls="navbarSupportedContent"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav me-auto mb-2 mb-lg-0">
          <li class="nav-item">
            <a class="nav-link active" aria-current="page" href="#">Home</a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="#">Link</a>
          </li>
        </ul>
        <ul class="navbar-nav mb-2 mb-lg-0">
          <li class="nav-item dropdown">
            <a
              class="nav-link dropdown-toggle"
              href="#"
              role="button"
              data-bs-toggle="dropdown"
              aria-expanded="false"
            >
              {{ userName }}
            </a>
            <ul class="dropdown-menu dropdown-menu-end">
              <li><a class="dropdown-item" href="#">Profile</a></li>
              <li><a class="dropdown-item" href="#">Logout</a></li>
            </ul>
          </li>
        </ul>
      </div>
    </div>
  </nav>


  <Posts
    :posts="posts"
    @increaseLikeCount="increaseLikeCount"
    @deletePost="deletePost"
    @addComment="addComment"
    @deleteComment="deleteComment"
  />
</template>

<style>
.cursor-pointer {
  cursor: pointer;
}

.very-low-opacity {
  opacity: 0.1;
}
</style>
