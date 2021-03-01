<style>
@charset "UTF-8";

/*
html5doctor.com Reset Stylesheet
v1.6.1
Last Updated: 2010-09-17
Author: Richard Clark - http://richclarkdesign.com
Twitter: @rich_clark
*/

html, body, div, span, object, iframe,
h1, h2, h3, h4, h5, h6, p, blockquote, pre,
abbr, address, cite, code,
del, dfn, em, img, ins, kbd, q, samp,
small, strong, sub, sup, var,
b, i,
dl, dt, dd, ol, ul, li,
fieldset, form, label, legend,
table, caption, tbody, tfoot, thead, tr, th, td,
article, aside, canvas, details, figcaption, figure,
footer, header, hgroup, menu, nav, section, summary,
time, mark, audio, video {
    margin:0;
    padding:0;
    border:0;
    outline:0;
    font-size:100%;
    vertical-align:baseline;
    background:transparent;
}

body {
    line-height:1;
}

article,aside,details,figcaption,figure,
footer,header,hgroup,menu,nav,section {
    display:block;
}

nav ul {
    list-style:none;
}

blockquote, q {
    quotes:none;
}

blockquote:before, blockquote:after,
q:before, q:after {
    content:'';
    content:none;
}

a {
    margin:0;
    padding:0;
    font-size:100%;
    vertical-align:baseline;
    background:transparent;
}

/* change colours to suit your needs */
ins {
    background-color:#ff9;
    color:#000;
    text-decoration:none;
}

/* change colours to suit your needs */
mark {
    background-color:#ff9;
    color:#000;
    font-style:italic;
    font-weight:bold;
}

del {
    text-decoration: line-through;
}

abbr[title], dfn[title] {
    border-bottom:1px dotted;
    cursor:help;
}

table {
    border-collapse:collapse;
    border-spacing:0;
}

/* change border colour to suit your needs */
hr {
    display:block;
    height:1px;
    border:0;  
    border-top:1px solid #cccccc;
    margin:1em 0;
    padding:0;
}

input, select {
    vertical-align:middle;
}
#app{
  padding: 50px;
}
h2{
  margin: 10px 0;
}
td,th{
  border: 0.5px solid black;
  padding: 5px;
  text-align: center;
}
th{
  background-color: burlywood;
}

</style>


<template>
  <div id="app">
    <h2>連絡先アプリ</h2>
    <h2>新規作成</h2>
    <form action="#">
      <ol>
        <li>
          <label>お名前：</label>
          <input type="text" name="name" v-model="nametext">
        </li>
        <li>
          <label>メールアドレス：</label>
          <input type="email" name="email" v-model="mailtext">
        </li>
      </ol>
      <button @click="newCreate">新規作成</button>
    </form>
    <h2>連絡先リスト</h2>
    <table>
      <tr>
        <th>ID</th>
        <th>NAME</th>
        <th>EMAIL</th>
        <th>UPDATE</th>
        <th>DELETE</th>
      </tr>
      <tr v-for="item in contactList" :key="item.id">
        <!-- v-forの使い方がまだだめ -->
        <td>{{item.id}}</td>
        <td>
          <input type="text" name="name" v-model="item.name">
        </td>
        <td>
          <input type="email" name="email" v-model="item.email">
        </td>
        <td>
          <button @click="updateButton(item.id,item.name,item.email)">更新</button>
        </td>
        <td>
          <button @click="deleteBUtton(item.id)">削除</button>
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
import axios from "axios"
export default {
  data(){
    return{
      nametext: "",
      mailtext: "",
      contactList:[]
    }
  },
  methods: {
    async getContent(){
      const reData = await axios.get("http://127.0.0.1:8000/api/contact/");
      this.contactList = reData.data.data;
      // なぜこれはdata.dataなのか？
    },
    async newCreate(){
      const sendData = {
        name: this.nametext,
        email: this.mailtext
      }
      await axios.post("http://127.0.0.1:8000/api/contact/",sendData)
      await this.getContent();
    },
    async updateButton(id,name,email){
      const sendData = {
        name: name,
        email: email
      };
      await axios.put("http://127.0.0.1:8000/api/contact/" + id, sendData);
      await this.getContent();
    },
    async deleteButton(id){
      await axios.delete("http://127.0.0.1:8000/api/contact/" + id);
      await this.getContent();
    }
  },
  created(){
    this.getContent();
  },
}
</script>

