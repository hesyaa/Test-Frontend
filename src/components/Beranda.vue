<template>
  <div id="body">
     <div class="container">
         <header>
             <a href="#" class="logo">Forum anak-IT</a>
             <nav>
                 <a href="#" class="hide-desktop">
                     <img src="../assets/ham.png" alt="toogle menu" class="menu" id="menu" v-on:click="mobile">
                 </a>
                 <ul v-bind:class="{hideMobile}" id="nav">
                     <li id="exit" class="exit-btn hide-desktop" v-on:click="exit">
                        <img src="../assets/ham.png" alt="exit menu" id="exitimg">
                    </li>
                    <li>
                        <input type="text" name="search" placeholder="search..." id="search" autocomplete="OFF">
                    </li>
                     <li class="dropdown">
                         <a href="#">Categories</a>
                         <div class="dropdownlist">
                             <a href="">Linux </a>
                             <a href="">Windows</a>
                             <a href="">Android</a>
                         </div>
                     </li>
                     <li><a href="#" v-on:click="login2">Login</a></li>
                     <li><a href="#" v-on:click="regis">Register</a></li>
                 </ul>
             </nav>
         </header>
     </div>
     <section>
         <article>
               <h2>Lampu webcam tiba tiba menyala sendiri</h2>
               <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Amet aspernatur quaerat obcaecati sunt corporis officia?</p>
               
               <fieldset>
                   <legend><h3>Komentar</h3></legend>
                   <div class="body" v-for="data in datas" v-bind:key="data.id">
                    <div class="comments">
                        <img v-bind:src="''+ data.avatar" width="160" height="100">
                        <span>
                            <h5>{{data.author}}</h5>
                            <p class="date">{{data.date}}</p>
                            <p>{{data.message}}</p>
                            <span id="points">{{data.point}}</span> <button id="arrow" class="point" v-on:click="thumbup(0)"><i class="fas fa-arrow-up"></i></button> <button id="arrow"> <i class="fas fa-arrow-down"></i></button>
                        </span>
                    </div>
                     <div class="reply" v-for="bales in balasan" v-bind:key="bales.id">
                         <img v-bind:src="''+ bales.avatar" width="100" height="100">
                        <span>
                            <h5>{{bales.author}}</h5>
                            <p class="date">{{bales.date}}</p>
                            <p>{{bales.message}}</p>
                             <span id="points">{{bales.point}}</span><button id="arrow" class="point" v-on:click="thumbup(1)"><i class="fas fa-arrow-up"></i></button> <button id="arrow"> <i class="fas fa-arrow-down"></i></button>
                        </span>
                     </div>
                  </div>
               </fieldset>
               <form action="/" @submit="addcomment">
                <fieldset>
                    <legend><h3>Tambahkan komentar</h3></legend>
                    <input type="text" name="name" id="name" placeholder="name" autocomplete="OFF" v-model="name">
                    <!-- <p v-bind:style="{color:'red'}">{{this.valid[0]}}</p> -->
                    <input type="email" name="email" id="email" placeholder="email" autocomplete="OFF" v-model="email"><br>
                     <!-- <p v-bind:style="{color:'red'}">{{this.valid[1]}}</p> -->
                    <textarea name="textarea" id="textarea" cols="50" rows="20" placeholder="komentar anda" autocomplete="OFF" v-model="komentar"></textarea><br>
                     <!-- <p v-bind:style="{color:'red'}">{{this.tellor}}</p> -->
                    <button type="reset" id="btnres">reset</button><button type="submit" id="btnsub">Submit</button>
                    <p v-for="valide in valid" v-bind:key="valide" v-bind:style="{color:'red', paddingLeft:'10px'}">{{valide}}</p>

                </fieldset>
                </form>
                <br><br>
         </article>

         <aside>
             <h4>Diskusi 5 teratas</h4>
             <div class="news">
                  <span id="box" style="fontsize:10px;">1</span> <p id="text"><a href="#">Bersihkan laptop dari butiran debu</a></p>
             </div>
             <div class="news">
                  <span id="box">2</span> <p id="text"><a href="#">Batas aman overclock PC rakitan</a></p>
             </div>
             <div class="news">
                  <span id="box">3</span> <p id="text"><a href="#">Cara mengetahui akun fb di hack lewat aplikasi</a></p>
             </div>
         </aside>
     </section>

     <div class="bgmodals" v-bind:class="{modalsactive}">
         <div class="modals">
             <form action="#" id="formlogin" @submit="login">
                 <label for="">email: </label><br>
                 <input type="email" name="logemail" id="email" placeholder="your email" v-model="logemail" autocomplete="Off"><br>
                 <label for="">password: </label><br>
                 <input type="password" name="password" id="logepass" placeholder="your pass" v-model="logepass" autocomplete="Off"><br><br>
                 <button type="submit" id="submitlog">Submit</button><br><br>
                 <p v-for="validlog in validlogs" v-bind:key="validlog" v-bind:style="{color:'red', paddingLeft:'10px'}" id="errlog">{{validlog}}</p>
             </form>
             <span class="mdclose" v-on:click="close">X</span>
         </div>
     </div>

     <div class="bgmodalregis" v-bind:class="{modalsregisactive}">
         <div class="modalregis">
            <form action="" id="mdregis" @submit="regisform">
                <label for="">name</label><br>
                <input type="text" name="name" id="mdregisname" placeholder="name" v-model="regisname"><br>
                <label for="">email</label><br>
                <input type="email" id="mdregisname" placeholder="email" v-model="regismail"><br>
                <label for="">password</label><br>
                <input type="password" id="logepass" placeholder="password" v-model="regispass"><br><br>
                <button type="submit" id="subregis">Register</button>
                <p v-for="errReg in errRegis" v-bind:key="errReg" v-bind:style="{color:'red', paddingLeft:'10px'}" id="errlog">{{errReg}}</p>
            </form>
             <span class="mdclose" v-on:click="mdrgclose">X</span>
         </div>
     </div>
   
  </div>
</template>

<script>
import json from '../comments.json';
export default {
    data(){
        return {
           errors: [],
           errlog: [],
           errRegis: [],
           valid: null,
           validlogs: null,
           validregis: null,
           name: null,
           email: null,
           komentar: null,
           modalsactive: false,
           modalsregisactive: false,
           logemail: null,
           logepass: null,
           regisname: null,
           regismail: null,
           regispass: null,
           hideMobile: false,
           datas: [],
           balasan: [],
           like: false,
           dislike: null,
           point: 0,
           score: 0,
        }
    },
    created(){
      const users =  axios.get('http://localhost:3000/body') //alamat http diload dari json-server yang membaca comments.json
       .then(res => {
           console.log(res.data.id)
           this.datas = res.data
           let users = res.data 
        users.forEach(user => {
            console.log(user.replies)
            this.balasan = user.replies
        })
       })
       
    },
    methods: {
       change: function(){
           this.replies = 'hello world'
       },
       addcomment: function(e){
           if(this.name && this.email && this.komentar){
               console.log('success add')
               this.errors.push('success')
               return true
           } this.errors = []

           if(!this.name){
               this.errors.push('nama harus ditulis')
               this.valid = this.errors
           
           }

           if(!this.email){
               this.errors.push('email harus ditulis')
               this.valid = this.errors
           }

           if(!this.komentar){
               this.errors.push('komentar harus ditulis')
               this.valid = this.errors
           }
           console.warn('errors adding form', this.errors)
           e.preventDefault()
       },

       login: function(e){
            if(this.logemail && this.logepass){
                this.errlog.push('success')
                this.errlog.pop()
                return true
           } this.errlog = []
           if(!this.logemail){
               this.errlog.push('email harus ditulis')
               this.validlogs = this.errlog
           }

           if(!this.logepass){
               this.errlog.push('password harus ditulis')
               this.validlogs = this.errlog
           }

           console.warn('errors adding form', this.errlog)
           e.preventDefault()
       },
       
       regisform: function(e){
            if(this.regisname && this.regisemail && this.regispass){
               console.log('success add')
               return true
           } this.errRegis = [] 
            if(!this.regisname){
               this.errRegis.push('name harus ditulis')
               this.validregis = this.errRegis
           }

            if(!this.regismail){
               this.errRegis.push('email harus ditulis')
               this.validregis = this.errRegis
           }

           if(!this.regispass){
               this.errRegis.push('password harus ditulis')
               this.validregis = this.errRegis
           }

           console.warn('errors adding form', this.errRegis)
           e.preventDefault()
       },

       login2: function(){
           this.modalsactive = true;
       },
       close: function(){
           this.modalsactive = false;
       },
       regis: function(){
           this.modalsregisactive = true
       },
       mdrgclose: function(){
           this.modalsregisactive = false
       },

       mobile: function(e){
       this.hideMobile = true
        e.preventDefault();
       },

       exit: function(e){
           this.hideMobile = false
       e.preventDefault()
       },

       thumbup: function(index){
         var btnpoint = document.querySelectorAll('.point')
         var i;
         for(i=0; i < btnpoint.length; i++){
             btnpoint[index].style.backgroundColor='green'
         }
         console.log(index)
        },
        increment: function(e){
            if(this.point == 0 && this.point < 2 && this.score !== 1){
                this.point =+ 1
            } else {
                return false
            }
            e.preventDefault()
        },
         decrement: function(e){
            if(this.point == 0 && this.point < 2){
                this.score =+ 1
            } else {
                return false
            }
            e.preventDefault()
        },
        coba: function(){
            alert('hello')
            var newbtn = document.getElementById('btnnew')
            newbtn.style.backgroundColor='teal'
        }
    },

    mounted(){

   }
}
</script>

<style scoped>
 #body {
     font-family: poppins;
 }

 #formlogin {
     border:  1px solid lightgray;
     width: 60%;
     height: auto;
     padding: 2em;
 }

 #search {
     display: flex;
     background-color: white;
     width: 350px;
     padding: .3em;
     margin-top: .5em;
     justify-content: center;
 }

 #mdregis {
     padding: 1em;
     width: 50%;
     border: 1px solid lightgrey;
 }

 #mdregisname {
     border-radius: 0;
     margin: 1em;
     background-color: white;
     border: 1px solid lightgrey;
 }

 #submitlog, #subregis {
     width: 50%;
 }

 #errlog {
     font-size: 12px;
 }

 .bgmodals {
     position: fixed;
     width: 100%;
     height: 100vh;
     top: 0;
     left: 0;
     background-color: rgb(0,0,0,0.5);
     display: flex;
     justify-content: center;
     align-items: center;
     visibility: hidden;
     opacity: 0;
     transition: visibility 0s, opacity 0.7s;
 }

  #logepass {
     background-color: white;
     border: 1px solid lightgray;
     width: 50%;
     border-radius: 0;
     margin: 10px;
     padding: .5em;
 }

 .modalsactive {
    visibility: visible;
    opacity: 1;
 }

 .bgmodalregis {
    position: fixed;
    top: 0;
    left: 0;
    background-color: rgb(0,0,0,0.5);
    height: 100vh;
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    visibility: hidden;
    opacity: 0;
    transition: visibility 0s, opacity 0.7s;
 }

 .modalregis {
     background: white;
     width: 60%;
     position: relative;
     padding: 1em;
     display: flex;
     justify-content: center;
     align-items: center;
     height: 60%;
     flex-direction: column;
 }

 .modalsregisactive {
      visibility: visible;
      opacity: 1;
 }

  label {
    text-align: left;
    /* border: 1px solid grey; */
    width: 80%;
    margin-top: 1em; 
    font-weight: bold;
    margin-left: 10px;
  }

.mdclose {
    position: absolute;
    top: 10px;
    right: 10px;
    color: white;
    cursor: pointer;
    background-color: rgb(216, 63, 63);
    /* border-radius: 50%; */
}

  .modals {
      position: relative;
      width: 70%;
      height: 50%;
      background-color: white;
      padding: 1em;
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;
  }

 .like {
     background-color: seagreen;
 }

 header {
     display: flex;
     justify-content: space-around;
     background-color: #71695f;
     border: none;
     height: 10vh;
 }

 nav ul {
     display: flex;
 }

 .date, #points {
     font-size: 10px;
 }

 #text {
     margin-left: 5px;
 }

 li a .dropdown {
     display: none;
 }

 .dropdownlist {
    display: none;
    position: absolute;
    background-color: #f9f9f9;
    min-width: 160px;
    box-shadow: 0px 8px 16px 0px rgba(0,0,0,0.2);
    z-index: 1;
  }

.dropdownlist a {
   color: black;
   padding: 12px 16px;
   text-decoration: none;
   display: block;
   text-align: left;
}

li.dropdown {
   display: inline-block;
}

.dropdown:hover .dropdownlist {
   display: block;
}

 #box {
     background-color: #71695f;
     color: white;
     margin-top: 5px;
     font-size: 10px;
 }

 #name, #textarea, #email {
     background-color: white;
     border: 1px solid lightgray;
     width: 70%;
     border-radius: 0;
     margin: 10px;
 }

 button[type=reset]{
     margin-left: 10px;
     padding: 10px;
     background-color: lightgray;
     border: none;
     border-radius: 10px;
     width: 10%;
 }

 button[type=submit]{
     background-color: #635c53;
     padding: 10px;
     border: none;
     margin-left: 4px;
     border-radius: 10px;
     color: white;
     width: 10%;
 }

 button:hover {
     cursor: pointer;
 }

 li a:hover {
     background-color:black;
     color: white;
 }

 .reply {
     padding-top: 1em;
     border: 1px solid lightgray;
     display: flex;
     width: 80%;
     margin-left: 10%;
     margin-bottom: 1em;
 }

 .form {
     padding: .5em;
     border: none;
 }

 h3 {
     margin-right: 10px;
 }

 #img {
     width: 50px;
     height: 60px;
 }

 fieldset {
     border: 1px solid lightgray;
     border-left: 0;
     border-right: 0;
     border-bottom: 0;
 }

 .comments {
     display: flex;
     /* border: 1px solid grey; */
 }

 span {
     padding: 10px;
 }

 h3 {
     margin-right: 30px;
 }

 p {
     font-size: 12px;
 }

 section {
     width: 80%;
     border: 1px solid lightgray;
     border-left: 0;
     border-right: 0;
     border-bottom: 0;
     border-top: 0;
     margin: auto;
     display: grid;
     grid-template-columns: 2fr 1fr;  
 }


 article {
     border: 1px solid lightgray;
     border-left: 0;
 }
 

 aside {
     padding-left: 1em;
     
 }

 .news {
     display: flex;
 }

 h5 {
    margin: 0;
 }

 ul li a, .logo {
    text-decoration: none;
    padding: .8em;
    font-size: 1em;
    color: white;
}

  input[type=text], input[type=email], textarea{
      width: 60%;
      height: 25px;
      margin-left: 10px;
      background-color: #dddddd;
      border: none;
      border-radius: 10px;
      padding: .4em;
  }

  *:focus {
      outline: none;
  }

 ul {
    list-style-type: none;
    display: flex;
 }

 #exit {
     display: none;
 }

 nav ul li ,.logo {
     display: flex;
     justify-content: space-around;
     align-items: center;
 }

 .hide-desktop {
     display: block;
 }

 #menu {
     position: absolute;
     top: 10px;
     right: 20px;
     display: none;
 }

 .logo {
     text-decoration: none;
     color:white
 }

 #arrow {
     border: none;
     padding: 5px;
 }

 #arrow:hover {
     background-color: grey;
 }

 @media only screen and (max-width: 1080px) {

     #btnres, #btnsub {
         width: 50%;
         margin: .5em;
     }

     section {
         padding: 1em;
         width: 100%;
         grid-template-columns: 2fr
     }

     .modals {
         width: 80%;
     }

     #logepass,#email {
         width: 80%;
     }

     .modalregis {
         width: 80%;
     }

     #mdregis {
         width: 70%;
     }
 }

 @media only screen and (max-width: 900px) {
     #btnres, #btnsub {
         width: 50%;
         margin: .5em;
     }

     section {
         padding: 1em;
         width: 100%;
         grid-template-columns: 2fr
     }

     .modals {
         width: 80%;
     }

     #logepass,#email {
         width: 80%;
     }

     .modalregis {
         width: 80%;
     }

     #mdregis {
         width: 70%;
     }
 }

 @media screen and (max-width: 800px){
     
 li a .dropdown {
     display: none;
 }

 .dropdownlist {
     display: none;
     position: absolute;
     background-color: #f9f9f9;
     min-width: 160px;
     box-shadow: 0px 8px 16px 0px rgba(0,0,0,0.2);
     z-index: 1;
}

.dropdownlist a {
     color: black;
     padding: 12px 16px;
     text-decoration: none;
     display: block;
     text-align: left;
}

li.dropdown {
     display: inline-block;
}

.dropdown:hover .dropdownlist {
     display: block;
}
    
     #menu {
         display: block;
         
     }

     .show-desk {
         display:none;
     }

     .hideMobile {
         display: block;
     }

     .activeShowdesk {
         display: block;
    }

     nav ul {
         position: fixed;
         top: -18px;
         right: 0;
         width: 50%;
         background-color: #71695f;
         height: 100%;
         z-index: 7;
         padding: 0;
         border-radius: 0;
         display: none;
     }

     nav ul li a {
         color: white;
         text-decoration: none;
         display: block;
         width: 100%;
         padding: 1em 2em;
         background-color: #71695f;
         transition: 2s;
    }
    

     #exit {
         float: right;
         padding: 1em;
         display: block;
         width:20%;
         margin-bottom: 2em;
     }

     #exitimg {
         background-color: white;
     }

     #exit:hover {
         cursor: pointer;
     }

     #search {
         margin-top: 2em;
         width: 70%;
         margin-left: 2em;
     }
 }


</style>