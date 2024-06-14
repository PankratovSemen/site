<template>
<button @click="cancel" class="button">Отменить заказ</button>
</template>

<script>
import axios from "axios";

const API_URL = "http://api.tastyeat.ru/";
export default {
    name: "CancelButtonOrder",
    props: {

        baskets: Number
    },
    methods:{
        cancel(){
            axios.post(API_URL + "api/Order/Cancel?basketId="+ this.baskets)
        }
    }
}
</script>

<style scoped>

</style>