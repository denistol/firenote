<template>
  <div id="app" class="row">
    <div class="sidebar col">
      <span @click="writeUserData(160)">item</span>
      <span>item</span>
      <span>item</span>
      <span>item</span>
      <span>item</span>
      <span>item</span>
      <span>item</span>
      <span>item</span>
      <span>item</span>
      <span>item</span>
      <span>item</span>
      <span>item</span>
      <span>item</span>
      <span>item</span>
      <span>item</span>
    </div>
    <div class="div content">
      content
    </div>
  </div>
</template>

<script>
import firebase from 'firebase';
import sha256 from 'sha256';
import config from '../firebase_config.js'
  // Initialize Firebase

  firebase.initializeApp(config);

export default {
  name: 'App',
  methods:{
    writeUserData:function(name) {
      let randId = sha256( (Math.random()*9999).toString() );
      firebase.database().ref('notes/'+randId).set({
        username: name,
      });
    }
  }
}
</script>

<style lang="scss">
*{
  font-family: sans-serif;
  margin: 0;
  padding: 0;
}
  #app{
    height: 100vh;
  }
  div{
    display: flex;
    flex-direction: column;
  }
  .row{
    flex-direction: row !important;
  }
  .col{
    flex-direction: column !important;
  }
  .sidebar{
    background-color: #eee;
    width: 20%;
    span{
      border-bottom: 1px solid rgb(219, 219, 219);
      cursor: pointer;
      padding: 5px;
      &:hover{
        background-color: rgb(94, 94, 105);
        color: #fff;
      }
    }
  }
  .content{
    padding: 5px;
    width: 80%;
    flex: 1;
  }
</style>