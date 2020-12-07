<template>
<div class="container-sm">
      <div class="d-flex justify-content-center mt-5 text-white">
        <div class="card bg-transparent border-dark" style="width: 60rem;">
          <div class="card-body ">
          <form @submit.prevent="add">
            <input type="hidden"  v-model="form.id">
            <input type="text" placeholder="Nama" v-model="form.name">
            <input type="date" placeholder="Ttl" v-model="form.ttl">
            <input type="text" placeholder="Sekolah" v-model="form.sekolah">
            <input type="text" placeholder="Kelas" v-model="form.kelas">
            <input type="text" placeholder="Jurusan" v-model="form.jurusan"><br><br>
            <button class="btn-primary btn-block btn-lg" type="submit" v-show="!updateSubmit">Add</button>
            <button type="button" v-show="updateSubmit" @click="update(form)">update</button>
          </form>
          </div>
       </div>
      </div>    

  <div>
      <div class="d-flex justify-content-center mt-5 text-">
        <div class="card bg-transparent border-dark" style="width: 40rem;">
          <div class="card-body ">
    <h2>Hasil</h2><hr>
    <table border="2" align="center">
     <tr>
       <th>Nama</th>
       <th>Ttl</th>
       <th>Sekolah</th>
       <th>Kelas</th>
       <th>Jurusan</th>
       <th></th>
     </tr> 
    <tr v-for="user in users" :key="user.id">
      <td>
        <span>{{user.name}}</span> &#160;
      </td>
      <td>  
        <span>{{user.ttl}}</span> &#160;
      </td>
      <td>  
        <span>{{user.sekolah}}</span> &#160;
      </td>
      <td>  
        <span>{{user.kelas}}</span> &#160;
      </td>
      <td>  
        <span>{{user.jurusan}}</span> &#160;
      </td>  
      <td>
        <button @click="edit(user)">Edit</button> || <button @click="del(user)">Delete</button>
      </td>
    </tr>
    </table>
        </div>
       </div>
      </div>    
  </div>
</div>  
</template>

<script>
/* eslint-disable */
import axios from 'axios'
export default {
  data(){
    return{
     form: {
       id: '',
       name: '',
       ttl: '',
       sekolah : '',
       kelas : '',
       jurusan: ''
     },
     users: '',
     updateSubmit: false
    }
  },
  mounted() {
    this.load()
  },
  methods: {
    load(){
        axios.get('http://localhost:3000/users').then(res =>{
        this.users = res.data
      }).catch ((err) => {
        console.log(err);
      })
    },
    add(){
      axios.post('http://localhost:3000/users/', this.form).then(res => {
        this.load()
        this.form.name = ''
        this.form.ttl = ''
        this.form.sekolah = ''
        this.form.kelas = ''
        this.form.jurusan = ''
      })
    },
    edit(user){
      this.updateSubmit = true
      this.form.id = user.id
      this.form.name = user.name
      this.form.ttl = user.ttl
      this.form.sekolah = user.sekolah
      this.form.kelas = user.kelas
      this.form.jurusan = user.jurusan
    },
    update(form){
      return axios.put('http://localhost:3000/users/' + form.id , {name: this.form.name,ttl: this.form.ttl,sekolah: this.form.sekolah,kelas: this.form.kelas,jurusan: this.form.jurusan}).then
      (res => {
        this.load()
        this.form.id = ''
        this.form.name = ''
        this.form.ttl = ''
        this.form.sekolah = ''
        this.form.kelas = ''
        this.form.jurusan = ''
        this.updateSubmit = false
      }).catch((err) => {
        console.log(err);
      })
    },
    del(user){
      axios.delete('http://localhost:3000/users/' + user.id).then(res => {
        this.load()
        let index = this.users.indexOf(form.name)
        this.users.splice(index,1)
      })
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
