  <template>
    <div>
      <div class="iq-top-navbar">
      <div class="iq-navbar-custom">
        <nav class="navbar navbar-expand-lg navbar-light p-0">
    
        <h1><router-link :to="{ name: 'home'}">Sevima Medsos </router-link></h1>
        <div v-on:click="toggleDropdown" id="menu-select" style="font-size:22px">
          +

        </div>
      </nav>
      </div>
      </div>
      <div v-if="showDropdown" id="menu" @click="toggleDropdown">
        <ul >
          <li><router-link :to="{ name: 'home'}">#home </router-link></li>
          <li v-if="isAuth"><router-link :to="{ name: 'user', params: {name:user.name, user_id:user.id} }">{{user.name}}</router-link></li>
          <li  v-if="isAuth"><router-link to="/user/profile">Settings</router-link></li>
          <li  v-if="isAuth"><a @click="logout">Logout</a></li>
          <li  v-if="!isAuth"><router-link to="/">Login</router-link></li>
         
        </ul>
    </div>
    <div id="content-page" class="content-page">
      <div class="container">
        <router-view>
        </router-view>
      </div>
      </div>
    </div>
  </template>
  <script>
  export default {
      data() {
          return{

            showDropdown:false
          }
        },
        mounted(){
          window.scroll(0,0);
        },
        methods:{
          logout(){
            localStorage.removeItem('token');
            this.$store.commit('user/setAuth', false);
          },
          switchLang(lang){
              localStorage.setItem('lang', lang);
              this.$i18n.locale=lang;
          },
          toggleDropdown(){
            if(this.showDropdown)
            {
              this.showDropdown=false
            }else {
              this.showDropdown=true;
            }
          }
        },
        computed:{

          user(){
            return this.$store.getters["user/getUser"];
          },
          isAuth(){
            return this.$store.getters["user/isAuth"];
          }
        }

      }
  </script>
  <style>

  </style>
