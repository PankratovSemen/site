<script setup>
    /**
  * ������ ���� ����/����� ����� ������������ � UTF-8
  */
    import { ref } from 'vue'
    import Counter from './child/Counter.vue'
    import Basket from './child/Basket.vue'
    const test = ref('change-count')

    const callback = data => test.value = data
    function addBuy(id) {
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


</script>

<template>
   
    <div class="grid-x grid-padding-x">
        <div class="cell medium-4 large-3" id="item1" v-for="(item,id) in item" :key="item.id">

            <img src="../img/1.jpg" alt="��� ����������� �� �����������" class="cardimage">
            <div class="small-12">
                <h1>{{item.title}}</h1>
                <p>{{content.DescriptionLabel}}</p>
                <p>{{item.description}}</p>
            </div>
            <div class="grid-x">
                <br />
                <div class="small-12" id="countstitle">{{content.CountLabel}}</div>
                <br />
                <br />
                <Counter @change-count="callback" />


                <br />
                <div class="medium-12">
                    <button id="addBuy" @click="addBuy(item.id)">{{content.addBuyRU}}</button>
                </div>
            </div>
        </div>



    </div>

</template>

<script>

    import emitter from 'tiny-emitter/instance'
    import { ref } from 'vue'
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
    const routes = [{ path: '/counter', component: Counter }]
    export default
        {
            components: {
                Counter,
                Basket
            },
            data() {
                return {

                    item: [],
                    content: {
                        addBuyRU:'�������� � �������',
                        DescriptionLabel:'��������',
                        CountLabel:'����������ss',
                    }


                }
            },


            methods: {
                async refreshData() {
                    axios.get(API_URL + "api/Product").then(
                        (response) => {
                            const data = response.data;
                            this.item = data;
                            

                        }
                    )

                },







            },

            created() {
                this.refreshData();
                this.timer = setInterval(this.refreshData, 10000);
            },
            mounted: function () {
                this.refreshData();


            },


        }





</script>