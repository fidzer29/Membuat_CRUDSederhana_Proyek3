<template lang="html">
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.1/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-iYQeCzEYFbKjA/T2uDLTpkwGzCiq6soy8tYaI1GyVh/UjpbCx/TYkiZhlZB6+fzT" crossorigin="anonymous">
<div class="">
  <h1 class="text-center">TUGAS MEMBUAT CRUD PROYEK 3</h1>
  <form @submit.prevent="save">
    <input type="hidden" v-model="form._id" name="_id" >
    <input type="text" class="form-control" placeholder="Input nama" v-model="form.ItemName" name="ItemName" ><br>
    <button type="submit" v-show="!updateSubmit" name="button" class="btn btn-primary">Save</button>
    <button type="button" v-show="updateSubmit" v-on:click="update(form)" name="button" class="btn btn-primary">Update</button>
  </form>

<br><h4 class="text-center">DATA USERS</h4></div>

<ul v-for="user in users" :key="user.id">
  <table class="table">
  <thead>
    <tr>
      <th scope="col">ID</th>
      <th scope="col">Name</th>
      <th scope="col">Action</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        <li>{{ user._id }} <br>
        </li>
      </td>
      <td>
        <li>{{ user.ItemName }} <br>
        </li>
      </td>
      <td>
        <li><button type="button" v-on:click="edit(user)" name="button" class="btn btn-success">Edit</button> ||
        <button type="button" v-on:click="hapus(user)" name="button" class="btn btn-danger">Delete</button>
        </li>
      </td>
    </tr>
  </tbody>
</table>
</ul>

</template>

<script>

import axios from 'axios'
import swal from 'sweetalert';

export default {
  data(){
    return{
      form:{
        _id: '',
        ItemName:'',
        __v: ""
      },
      users: [],
      updateSubmit: false
    }
  },

  methods:{
    load(){
      axios.get('http://10.10.131.46:3000/get-all')
      .then((res)=>{
        this.users = res.data
        // console.log(res.data)
      })
      .catch(()=>{
        alert('eror load data')
      })
    },

    save(){
      axios.post('http://10.10.131.46:3000/create', this.form)
      .then(()=>{
        this.load()
        this.form.ItemName = ''
        swal("Data Tersimpan");
      })
      .catch(()=>{
        swal("Data Error");
      })
    },
    edit(user){
      this.updateSubmit = true
      this.form._id = user._id
      this.form.ItemName = user.ItemName
    },
    update(form){
      axios.put('http://10.10.131.46:3000/ItemName/' + form._id , {
        ItemName: this.form.ItemName
      })
      .then((res)=>{
        this.load()
        this.form._id = ''
        this.form.ItemName = ''
        this.updateSubmit = false
        console.log("ini resnya : ",res);
        swal("Data Update");
      })
      .catch(()=>{
        swal("Update Error");
      })
    },
    hapus(user){
      axios.delete('http://10.10.131.46:3000/ItemName/' + user._id)
      .then(()=>{
        this.load()
        this.form.ItemName = ''
        swal("Data Terhapus");
      })
      .catch(()=>{
        swal("Data Error");
      })
    }
  },

  mounted() {
    this.load()
  }
}

</script>

<style lang="css" scoped>

</style>