<script setup>
import { ref, computed, watch, provide } from 'vue'
import moment from 'moment'
import Posts from './components/Posts.vue'
import CreatePost from './components/CreatePost.vue'
import LayoutContent from './components/LayoutContent.vue'

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

function increaseLikeCount(index) {
  posts.value[index].likes++
}

function deletePost(index) {
  posts.value.splice(index, 1)
}

function postCreate(post) {
  posts.value.push(post)
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

provide('posts', posts)

const currentComponent = ref('Posts')

</script>

<template>
  <LayoutContent :userName="userName" @changeComponent="(component) => (currentComponent = component)" :currentComponent="currentComponent" />
  <CreatePost v-if="currentComponent === 'CreatePost'" @postCreate="postCreate" :postCount="posts.length" class="bg-light" />
  <Posts
    v-else-if="currentComponent === 'Posts'"
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
