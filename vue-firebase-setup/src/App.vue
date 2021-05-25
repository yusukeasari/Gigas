<template>
  <div id="nav">
    <router-link to="/">Home</router-link> |
    <router-link to="/about">About</router-link>
  </div>
  <router-view/>
</template>
<script>
import firebase from "@/firebase/firebase";
export default {
  name: "Home",
  data() {
    return {
      contents: [],
      db: null
    };
  },
  created() {
      this.db = firebase.firestore();
      this.syncContent();
  },
  methods: {
    syncContent() {
      const category = "main"; // normal カテゴリーのみを対象とする
      this.contents.splice(0); // クリア
      this.db
        .collection("gigas-contents")
        .where("category", "==", category)
        .get()
        .then(querySnapshot => {
          console.log("querySnapshot",querySnapshot)
          querySnapshot.forEach(doc => {
            console.log("data?",doc.data().body);
            this.contents.push(doc.data());
          });
        }, this.contents)
        .catch(function(error) {
          console.log("ERROR: ", error);
        });
    }
  }
};
</script>
<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
