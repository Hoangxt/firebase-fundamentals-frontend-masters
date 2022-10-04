<script setup>
import { onMounted, reactive, onBeforeMount } from 'vue';
import { useRouter } from 'vue-router';

import { initializeApp } from 'firebase/app';
import {getFirestore, collection,onSnapshot,doc,setDoc} from 'firebase/firestore';
import {config} from '../config'

const firebaseApp = initializeApp(config.firebase);
const firestore = getFirestore(firebaseApp);
const markdownsCol = collection(firestore, 'markdowns');

const state = reactive({ markdowns: [] });
const router = useRouter();

onBeforeMount(async () => {
  // Get a user
})

onMounted(() => {
  onSnapshot(markdownsCol,snapshot => { 
    state.markdowns = snapshot.docs.map(doc => ({ id: doc.id, ...doc.data() }));
  })
})

function newMarkdown() {
  const newId = Math.random().toString(36).substr(2, 5);
  router.push(`/editor/${newId}`)
}
</script>

<template>
  <h1>I am the dashboard</h1>

  <ul v-for="markdown in state.markdowns" :key="markdown.id">
    <li>
      <router-link :to="{ path: `/editor/${markdown.id}` }">{{ markdown.id }}</router-link>
    </li>
  </ul>

  <button @click="newMarkdown">New</button>

</template>
