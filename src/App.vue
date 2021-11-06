<template>
<div style="border:none;border-radius: 20px;background-color: aqua;width: 800px;height: 300px;margin-left: 300px;margin-top: 50px;">
  <div>
    <h1 style="text-align: center;font-family:verdana;">FORM PEMINJAMAN BUKU</h1>
    <form @submit.prevent="add">
        <center>
      <label  style="padding: 10px;margin-left: -20px;font-family: sans-serif;" for="">Nama Siswa<font style="margin-left:65px;">:</font></label>
      <input required placeholder ="Masukkan Nama Siswa" style="width: 200px;margin-top: 10px;" type="text" name="nama_siswa" v-model="form.name" /><br>
      <label  style="padding: 10px;padding-right: 15px;margin-left:-18px;font-family: sans-serif;" for="">Judul Buku<font style="margin-left: 75px;">:</font></label>
      <input required placeholder ="Masukkan Judul Buku" style="width: 199px;margin-top: 10px;margin-left: -5px;" type="text" name="judul_buku" v-model="form.judulbuku" /><br>
      <label  style="padding: 5px; padding-right: 6px;margin-left:-18px;font-family: sans-serif;" for="">Tanggal Pinjam<font style="margin-left: 50px;">:</font></label>
      <input required placeholder ="Masukkan Tanggal Pinjam" style="width: 199px;margin-top: 10px;margin-left: 5px;" type="text" nmae="tanggal_pinjam" v-model="form.tanggalpinjam" /><br>
      <label  style="padding: 18px;margin-left: -35px;font-family: sans-serif;" for="">Tanggal Pengembalian:</label>
      <input required placeholder ="Masukkan Tanggal Pengembalian" style="width: 200px;margin-top: 10px;margin-left:-10px;" type="text" name="tanggal_pengembalian" v-model="form.tanggalpengembalian"/><br />
        <button style="background-color: darkblue;margin-top: 30px;font-family: sans-serif;" type="submit" v-show="!updateSubmit"><font style="font-size: 15px;font-family: Calibri;color:white;">Add Peminjaman</font></button
        ><button style="background-color: darkblue;margin-top: 30px;font-family: sans-serif;" type="button" v-show="updateSubmit" @click="update(form)">
        <font style="font-size: 15px;font-family: Calibri;color:white;">Perpanjangan</font>
        </button></center>
        <br /><br />
      </form>
  </div>
  <div style="margin-top: 50px;font-family: sans-serif;">
  <marquee style="margin-left: 70px;" behavior="scroll" direction="up" scrollamount="2"><h1>Selamat Datang Di Form Peminjaman Buku</h1></marquee>
  </div>
  <div style="border:none;border-radius: 10px;background-color:aqua;width: 800px;height: 230px;margin-top: 1=90px;">
    <h1 style="font-family:verdana;font-size: 30px;text-align:center;">TABEL DAFTAR BUKU</h1>
    <center><table border='5' style="padding: 1px;background-color: white;width: 800px;height: 150px;margin-top: 40px;" >
      <thead>
      <tr style="margin-left: 100px;border: 5px solid black;background-color:yellow;">
        <th>No</th>
        <th>Nama Siswa</th>
        <th>Judul Buku</th>
        <th>Tanggal Pinjam</th>
        <th>Tanggal Pengembalian</th>
        <th>Aksi</th>
      </tr>
      </thead>
      <tbody>
    <tr style="border: 5px solid black;background-color:orangered" v-for="user in users" :key="user.id">
      <th>{{user.id}}</th>
      <th>{{user.name}}</th>
      <th>{{user.judulbuku}}</th>
      <th>{{user.tanggalpinjam}}</th>
      <th>{{user.tanggalpengembalian}}</th>
      <th>
       <span style="border:1px solid black;background-color:green;color:white;border-height:50px;" @click="edit(user)">Perpanjangan</span> |  <span style="border:1px solid black;background-color:red;color:white;" @click="del(user)">Kembalikan</span>
      </th>
    </tr>
      </tbody>
    </table>
    </center>
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
          judulbuku: '',
          tanggalpinjam: '',
          tanggalpengembalian: ''
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
        axios.get('http://localhost:3000/users').then(res => {
        this.users = res.data
      }).catch ((err) => {
        console.log(err);
        
      })
    },
      add(){
      axios.post('http://localhost:3000/users/', this.form).then(res => {
          this.load()
          this.form.name = ''
      })
    },
    edit(user){ 
        this.updateSubmit = true
        this.form.id = user.id 
        this.form.name = user.name 
        this.form.judulbuku = user.judulbuku 
        this.form.tanggalpinjam = user.tanggalpinjam 
        this.form.tanggalpengembalian = user.tanggalpengembalian 
    },
    update(form){ 
       return axios.put('http://localhost:3000/users/' +form.id, {
          name: this.form.name,
          judulbuku: this.form.judulbuku,
          tanggalpinjam: this.form.tanggalpinjam,
          tanggalpengembalian: this.form.tanggalpengembalian,
        }).then(res => {
        this.load()
        this.form.id = ''
        this.form.name = ''
        this.form.judulbuku  = ''
        this.form.tanggalpinjam  = ''
        this.form.tanggalpengembalian  = ''
        this.updateSubmit = false
      }).catch((err) => {
        console.log(err);
        
      })
    },
    del(user){
      axios.delete('http://localhost:3000/users/' + user.id).then(res =>{
          this.load()
          let index = this.users.indexOf(form.name)
          this.users.splice(index,1)
      })
    }
  }
}
</script>