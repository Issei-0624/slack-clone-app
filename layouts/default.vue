<template>
  <div class="app-layout">
    <div class="sidebar">
      <p>チャンネル一覧</p>
      <p v-for="channel in channels" :key="channel">
        <nuxt-link :to="`/channels/${channel.id}`">
          {{ channel.name }}</nuxt-link
        >
      </p>
    </div>
    <div class="main-content">
      <Nuxt />
    </div>
  </div>
</template>

<script>
import { db } from '~/plugins/firebase'

export default {
  data() {
    return {
      channels: [],
    }
  },
  mounted() {
    db.collection('channels')
      .get()
      .then((querySnapshot) => {
        querySnapshot.forEach((doc) => {
          this.channels.push({ id: doc.id, ...doc.data() })
        })
      })
  },
}
</script>

<style>
.app-layout {
  display: flex;
}

.sidebar {
  width: 300px;
  background: #4a4141;
  height: 100vh;
  padding: 20px;
}

.sidebar p {
  color: #ddd;
  padding-top: 4px;
}
.sidebar a {
  color: #ddd;
}
.main-content {
  width: 100%;
  background: #f1f1f1;
  height: 100vh;
}
</style>
