<!--<script setup>
    import { ref } from 'vue'
    
    const test = ref('change-count')

    const callback = data => test.value = data
    function addBuy(id) 
    {
        alert(test.value);
        const formData = new FormData();
        formData.append("Id", id);
        if (getCookie("session") == undefined) {
            axios.post(API_URL + "api/Basket/api/Add?products=" + id + "&count=" + test.value).then(
                (response) => {
                
                    setCookie("session", response.data, { secure: true, 'max-age': 86300 });
                    alert(response.data)
                }
            )

        }
        else if (getCookie("session") != undefined) {
            axios.post(API_URL + "api/Basket/api/Add?products=" + id + "&sessionId=" + getCookie("session") + "&count=" + test.value).then(
                (response) => {
                    if (response.data != getCookie("session")) {
                        setCookie("session", response.data, { secure: true, 'max-age': 86300 });
                    }
               
                }
            )
            alert("Ok");
        }
    }
    
    
</script>-->

<template>
    
    
    <div class="grid-x grid-padding-x topbar">

        <div class="large-6 cell">
            <div class="title-bar-left">
                <img src="./img/Menu.png" data-bs-toggle="offcanvas" data-bs-target="#offcanvasScrolling" class="menu" @click="open">
            </div>


        </div>
        <Basket/>
    </div>
   <div class="grid-x" v-if="isVisible">
       <div class="right-menu cell medium-5">
           <div class="grid-y">
               <div class="link">
                   <router-link to="/" exact>Главная</router-link>
                   <div v-if="gettersAuthData.role!=''">
                       <router-link to="/basketuser" exact>Корзина </router-link>

                   </div>
                   <div v-if="gettersAuthData.role==''">
                       <router-link to="/basket" exact>Корзина </router-link>
                   </div>

                   <div v-if="gettersAuthData.role=='Администратор' || gettersAuthData.role=='Менеджер' || gettersAuthData.role=='Кухонный работник'">
                       <router-link to="/orderPanel" exact>Заказы </router-link>
                       <br>
                       <router-link to="/Users/Create" exact>Создать пользователя </router-link>
                       <br>
                       <router-link to="/Products/Add" exact>Добавить продукт </router-link>
                   </div>
                   <div v-if="gettersAuthData.role=='Курьер'">
                       <router-link to="/Courier" exact>Мои доставки </router-link>
                   </div>
               </div>
           </div>
       </div>
   </div>
    <br>
    <br>
    <br>
    <br>
    <router-view lang="ru" ></router-view>

    
</template>

<script>

import { ref } from 'vue'
    import Basket from './components/child/Basket.vue'
    import {mapGetters} from 'vuex';
    const API_URL = "http://api.tastyeat.ru/"
    function getCookie(name) {
        let matches = document.cookie.match(new RegExp(
            "(?:^|; )" + name.replace(/([\.$?*|{}\(\)\[\]\\\/\+^])/g, '\\$1') + "=([^;]*)"
        ));
        return matches ? decodeURIComponent(matches[1]) : undefined;

    }
    function setCookie(name, value, attributes = {}) {

        attributes = {
            path: '/',
            // add other defaults here if necessary
            ...attributes
        };

        if (attributes.expires instanceof Date) {
            attributes.expires = attributes.expires.toUTCString();
        }

        let updatedCookie = encodeURIComponent(name) + "=" + encodeURIComponent(value);

        for (let attributeKey in attributes) {
            updatedCookie += "; " + attributeKey;
            let attributeValue = attributes[attributeKey];
            if (attributeValue !== true) {
                updatedCookie += "=" + attributeValue;
            }
        }

        document.cookie = updatedCookie;
    }
   
    export default
        {
            components: {

                Basket
            },
            data(){
                return{
                    isVisible:false
                }
            },
            methods:{
                open(){
                    if (this.isVisible==false){
                        this.isVisible=true
                    }
                    else if(this.isVisible==true){
                        this.isVisible = false
                    }
                }
            },
            computed: {
                ...mapGetters('auth', {
                    gettersAuthData: 'getAuthData',
                    getterLoginStatus:'getLoginStatus'
                })},
       
            

            

            
    }





</script>
<style scoped>

#offCanvas{
    visibility: visible;
}
    .topbar {
        position: fixed;
        width: 100%;
        z-index: 9999;
    }

    .right-menu {
        background-color: #817878;
        margin-top:80px;
        position: fixed;
        display: block;
        height: 100%;
        min-height:300px;
        max-height:1000px;
        z-index: 100;
    }
router-link{
    font-size:20px;
    color:white
}
.link{
    text-align:center;
    font-size:30px;
    font-family: "Segoe UI";
    color: white;

}
a{
    color:white;
}
</style>


