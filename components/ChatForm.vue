<template>
  <div class="input-container">
    <textarea
      v-model="text"
      @click="openLoginModal"
      @keydown.enter="addMessage"
    ></textarea>
    <el-dialog title="" :visible.sync="dialogVisible" width="30%">
      <div class="image-container">
        <img src="~assets/google_sign_in.png" @click="login" />
      </div>
    </el-dialog>
  </div>
</template>

<script>
import Vue from 'vue'
import ElementUI from 'element-ui'
import { db, firebase } from '~/plugins/firebase'
import 'element-ui/lib/theme-chalk/index.css'
Vue.use(ElementUI)
export default {
  data() {
    return {
      dialogVisible: false,
      text: null,
    }
  },
  methods: {
    openLoginModal() {
      this.dialogVisible = true
    },
    addMessage(event) {
      if (this.enterJapaneseConversion(event)) {
        return
      }
      const channelId = this.$route.params.id
      db.collection('channels')
        .doc(channelId)
        .collection('messages')
        .add({ text: this.text, createdAt: new Date().getTime() })
        .then(() => {
          this.text = null
        })
    },
    enterJapaneseConversion(event) {
      const codeForConversion = 229
      return event.keyCode === codeForConversion
    },
    login() {
      const provider = new firebase.auth.GoogleAuthProvider()
      firebase
        .auth()
        .signInWithPopup(provider)
        .then((result) => {
          const user = result.user
          console.log(user)
          this.dialogVisible = false
        })
        .catch((error) => {
          window.alert(error)
        })
    },
  },
}
</script>
<style scoped>
.input-container {
  padding: 10px;
  height: 100%;
}

textarea {
  width: 100%;
  height: 100%;
}

.image-container {
  display: flex;
  justify-content: center;
}

img {
  width: 70%;
  cursor: pointer;
}
</style>
