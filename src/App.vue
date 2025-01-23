

<template>
  
  <div class="heder">
    <h2>Заголовок страницы {{ info_page }}</h2>
    <div class="menu">
      <div 
        class="menuItem" 
        :class="{
        'menuItemCurrent': cur_page === page.id  // если условие истино, применяется название класса
        }"
        v-for="page in pages" 
        :key="page.id"
        @click="goto_page(page)"
        >
          <img :src="page.ico">
          <p>{{ page.title }}</p>
      </div>
    </div>
  </div>
  
  <div class="body"> 
    <div  
      v-if="cur_page === 1"  
    >
      <h3>Коктейли</h3>
      <p v-if="info_cocktail !=''">Готовится коктейль: {{ info_cocktail }} ...</p>
      <div class="menu">
        <div 
          class="menuItem" 
          v-for="cocktail in cocktails" 
          :key="cocktail.id"
          @click="make_cocktail(cocktail)"
          >
            <img :src="cocktail.ico">
            <p>{{ cocktail.title }}</p>
        </div>
      </div>
    </div>
    <div 
      v-if="cur_page === 2"
    >
    <h3>Напитки</h3>
      <p v-if="info_drink !=''">Выполняю: {{ info_drink }} ...</p>
      <div class="menu">
        <div 
          class="menuItem" 
          v-for="drink in drinks" 
          :key="drink.id"
          @click="make_drink(drink)"
          >
            <img :src="drink.ico">
            <p>{{ drink.title }}</p>
            
        </div>
      </div>
    </div>
    <div v-if="cur_page === 3">
      <h3>Настройка параметров подключения Двигателя</h3>
        <div>
            <span>DIR ВКлючить</span>
            <my-button @click="test(1)">ВКлючить</my-button>
            <span>DIR ВЫКлючить</span>
            <my-button @click="test(0)">ВЫКлючить</my-button>        
        </div>
    </div>
    <div 
      v-if="cur_page === 4"
    >
      <h3>Настройка Настройки МК</h3>
      
      <div class="menu">
        <div 
          class="menuItem" 
          v-for="drink_setting in drink_settings" 
          :key="drink_setting"
          
          >
            <span>{{ drink_setting.title }}</span>
            <input v-model="drink_setting.value" />
        </div>
        <my-button @click="save_drink_variable">Сохранить</my-button>
      </div>      
    </div>
    <div 
      v-if="cur_page === 5"
    >
      <h3>Настройка WiFi</h3>
      <div>
        
        <span>WiFi сеть:</span>
        <input v-model="wifi_setting.ssid" />
        <span>Пароль:</span>
        <input v-model="wifi_setting.pwd" />
        <div>
          <my-button @click="wifi_scan">Сканировать сети</my-button>
          <my-button @click="wifi_save">Сохранить</my-button>
        </div>
        <span>Выбрать WiFi из списка:</span>
        <div 
            v-if="wifi_network_visible === true"
            class="menuItem" 
            v-for="wifiscan_cur in wifiscan" 
            :key="wifiscan_cur"
            
            >
              <div @click="wifi_set(wifiscan_cur.ssid)">
                <div>Сеть: {{ wifiscan_cur.ssid }} </div>
                <div>Уровень: {{ wifiscan_cur.level }} </div>
                <div>Mac адрес: {{ wifiscan_cur.mac_adress }} </div>
              </div>
          </div>        
      </div>
    </div>
  </div>  
  <div class="footer">
    Футер 

 <!--   <div
      :class="{
        'class-name': page === pageNumber  // если условие истино, применяется название класса
      }"
    >
    </div>
-->    
  </div>
  
</template>

<script>
  import axios from "axios";

  

  export default{

    data(){
      return {
        info_page: 'Коктейли',
        info_cocktail: '',
        info_drink: '',
        cur_page: 1, //
        wifi_setting: [],
        wifiscan: [],
        wifi_network_visible: false,
        pages: [],
        cocktails: [],
        drinks: [],    
        drink_settings: [],         
        posts: [],
        dialogVisible: false, //
        isPostsLoading: false, //
       
      }
    },
    methods: {
      make_drink(drink){
        this.info_drink = drink.title
        axios.get('/do_step', {
            params: {
              action: drink.chapter
            }
          })
          .then(function (response) {
            console.log(response);
          })
          .catch(function (error) {
            console.log(error);
          })
          .then(function () {
            // always executed
          });        
      },

      wifi_scan(){
        this.get_wifi_scan()
        this.wifi_network_visible = true
      },
      
      wifi_set(wifi_ssid){
        this.wifi_setting.ssid = wifi_ssid
      },

      make_cocktail(cocktail){
        this.info_cocktail = cocktail.title
        axios.get('/make_cocktail', {
            params: {
              cocktail: cocktail.id
            }
          })
          .then(function (response) {
            console.log(response);
          })
          .catch(function (error) {
            console.log(error);
          })
          .then(function () {
            // always executed
          });         
      },
      goto_page(page){
        this.info_page = page.title
        this.cur_page = page.id
      },

      async get_mk_params(){
        
        try {
          const response = axios.get('./src/assets/base_cfg/mk_params_cfg.json')
          console.log(response)
          this.drink_settings = (await response).data
        } catch (e) {
          alert('Ошибка')
        }
         
        
      },
      async get_menu_pages(){
        
        try {
          const response = axios.get('./src/assets/ui_cfg/menu_pages_cfg.json')
          console.log(response)
          this.pages = (await response).data
        } catch (e) {
          alert('Ошибка')
        } 
      },
      async get_cocktails(){
        
        try {
          const response = axios.get('./src/assets/data_cfg/cocktails.json')
          console.log(response)
          this.cocktails = (await response).data
        } catch (e) {
          alert('Ошибка')
        } 
      },
      async get_drinks(){
        
        try {
          const response = axios.get('./src/assets/data_cfg/drinks_cfg.json')
          console.log(response)
          this.drinks = (await response).data
        } catch (e) {
          alert('Ошибка')
        } 
      },
      async get_wifi_setting(){
        
        try {
          const response = axios.get('./src/assets/base_cfg/wifi_cfg.json')
          console.log(response)
          this.wifi_setting = (await response).data
        } catch (e) {
          alert('Ошибка')
        } 
      },

      async get_wifi_scan(){
        
        try {
          const response = axios.get('/wifiscan')
          console.log(response)
          this.wifiscan = (await response).data
        } catch (e) {
          alert('Ошибка')
        } 
      },     
      
      wifi_save(){
        axios.get('/wifi_save', {
            params: {
              ssid: this.wifi_setting.ssid,
              pwd: this.wifi_setting.pwd
            }
          })
          .then(function (response) {
            console.log(response);
          })
          .catch(function (error) {
            console.log(error);
          })
          .then(function () {
            // always executed
          }); 
      },        

      async test(dir_value){
        console.log(dir_value)
        axios.get('/step_drive', {
            params: {
              dir: dir_value
            }
          })
          .then(function (response) {
            console.log(response);
          })
          .catch(function (error) {
            console.log(error);
          })
          .then(function () {
            // always executed
          });        
      },

      
      
      async save_drink_variable(){

       axios.get('/save_drink_variable', {
            params: {
              alkohol_drink_counter: this.drink_settings[7].value,
              sugar_drink_counter: this.drink_settings[8].value,
              position_ziro: this.drink_settings[0].value,
              position_vodka: this.drink_settings[1].value,
              position_gin: this.drink_settings[2].value,
              position_rome: this.drink_settings[3].value,
              position_likerblue: this.drink_settings[4].value,
              position_whisky: this.drink_settings[5].value,
              position_tequila: this.drink_settings[6].value
            }
          })
          .then(function (response) {
            console.log(response);
          })
          .catch(function (error) {
            console.log(error);
          })
          .then(function () {
            // always executed
          });

      },  

      async save_wifi_setting(){

      axios.get('/save_wifi_setting', {
          params: {
            ssid: this.wifi_setting.ssid,
            password: this.wifi_setting.pwd
          }
        })
        .then(function (response) {
          console.log(response);
        })
        .catch(function (error) {
          console.log(error);
        })
        .then(function () {
          // always executed
        });

      },  

          

      async save_mk_params(){

      }                                        

    },
    mounted(){

      this.get_mk_params()
      this.get_menu_pages()
      this.get_cocktails()
      this.get_drinks()
      this.get_wifi_setting()
      
    }

  
  }

</script>


<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}



body{
  font-family: Verdana, Helvetica, sans-serif;
  text-align: left;
  background-color: #222;
  margin: 0;
  color: #fff;
}

.heder{
  height: 100%; /* Высота */ 
  background: #342929;
  margin: 5px;
  padding: 5px;
  border: 3px solid #027428;
}



.menu {
 
  background: #342929;
  margin: auto;
  padding: 5px;
  border: 3px solid #027428;
  text-align: center;
  display: flex;
  flex-flow: row wrap;
}

.menuItem {
  align-items: center;
  border: 3px solid #027428;
  border-radius: 12px;
  background: #027428;
  margin: 2px;
  padding: 10px;
}

.menuItemCurrent {
  align-items: center;
  border: 3px solid #0b07ec;
  border-radius: 12px;
  background: #caebd5;
  margin: 2px;
  padding: 10px;
}

.body {
  height: 100%; /* Высота */ 
  background: #342929;
  margin: 5px;
  padding: 5px;
  border: 3px solid #027428;
}

.footer {
  height: 100%; /* Высота */ 
  background: #342929;
  margin: 5px;
  padding: 15px;
  border: 3px solid #027428;
}

</style>
