<template>



  <div id="app" class="col">
    <div class="header">
      header
    </div>
    <div class="row wrapper">
        <div class="sidebar col" v-if="loadedItems">
          <span @click="body = v.body" style="font-size:13px;" v-for="(v,i) in loadedItems" :key="i">
            {{v}}
            <img class="icn" src="./assets/del-w.svg" width="15px">
          </span>
        </div>
        <div class="div content">
          <textarea v-model="body"></textarea>
            <button @click="writeData">create note</button>
        </div>

      </div>


</div>

</template>

<script>
import firebase from 'firebase';
import sha256 from 'sha256';
import config from '../firebase_config.js'



firebase.initializeApp(config);

export default {
  name: 'App',
  data(){
    return {
      loadedItems : [],
      body:'',
    }
  },
  computed:{
  },
  methods:{
    writeData:function() {
      let randId = + new Date();
      let noteBody = {
        time: + new Date(),
        name: 'unknown',
        body: this.body,
      }
      firebase.database().ref('notes/'+randId).set(noteBody).then(data=>{
        console.log('added!');
        this.body = '';
      })
    },
    updateData:async function(){
      let ref = firebase.database().ref("notes/")
      ref.orderByKey().on("value", function(snapshot) {
      });

      this.loadedItems = await ref.once("value").then(data=>data.val());
    },
    watchData(){
      let that = this;
      let ref = firebase.database().ref("notes/")
      ref.orderByKey().on("value",function(data){
        that.loadedItems = data.val();
      })
    },
    deleteData(){
      let ref = firebase.database().ref("notes/2ce62fbb48334ac7edcdc1dd2d5c3e9db62c4f0fd6fabc06b90e96f44b1f5a58");
      ref.remove().then(res=>console.log(res));
    }
  },
  mounted(){
    this.updateData();
    this.watchData();
  },
}
</script>

<style lang="scss">
@mixin pad{
  padding: 14px;
}
$dark : #323d4a;
$light: #e1e4e7;
*{
  font-family: sans-serif;
  margin: 0;
  padding: 0;
  color: $dark;
  box-sizing: border-box;
}
textarea{
  display: flex;
  width: 100%;
  @include pad;
}
body{
      max-height: calc(100vh - 100px);
}
  #app{
    height:100%;
      max-height: calc(100vh - 100px);
    
  }
  .icn{
    margin-left:6px;
  }
  .header{
    background-color: $dark;
    @include pad;
    color: $light;
    height: 60px;
    justify-content: center;
    
  }
  .wrapper{
    position: absolute;
    top: 60px;
    height: calc(100vh - 60px);
    width: 100%;
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
    overflow-y: scroll;
    background-color: #eee;
    height: 100%;
    span{
      @include pad;
      justify-content: space-between;
      min-width: max-content;;
      min-height: 25px;
      display: flex;
      align-items: center; 
      border-bottom: 1px solid rgb(219, 219, 219);
      cursor: pointer;
      &:hover{
        background-color: $dark;
        color: $light;
      }
      img{
        visibility: hidden;
      }
      &:hover img{
        visibility: visible;
      }
    }
  }
  .content{
    @include pad;
    flex: 1;
    }
</style>