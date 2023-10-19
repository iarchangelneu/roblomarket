<template>
    <div class="page">
        <h1>Транзакция прошла успешно</h1>
    </div>
</template>
<script>
import global from '~/mixins/global';
import axios from 'axios';
export default {
    mixins: [global],
    data() {
        return {
            pathUrl: 'https://roblomarket.kz',
        }
    }
    ,
    methods: {
        sendRequest(reference) {
            const token = this.getAuthorizationCookie();
            const path = `${this.pathUrl}/api/money/success/${reference}`;
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            axios
                .get(path)
                .then(response => {
                    console.log(response)
                    if (response.status == 200) {
                        //  window.location.href = '/'
                    }
                })
                .catch(error => {
                    console.log(error);
                });
        },
    },
    mounted() {
        const url = window.location.href;
        const match = url.match(/order_pay_roblomarket_(\d+)/);

        if (match) {
            this.extractedValue = match[0];
            console.log(this.extractedValue);

            this.sendRequest(match[0])
        }
    },
}
</script>
<script setup>
useSeoMeta({
    title: 'Успешная транзакция | RobloMarket',
    ogTitle: 'Успешная транзакция | RobloMarket',
    description: 'Успешная транзакция | RobloMarket',
    ogDescription: 'Успешная транзакция | RobloMarket',
})
</script>
<style lang="scss" scoped>
.page {
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;

    h1 {
        font-size: 40px;
        font-family: var(--unb);
        color: #000;
        font-weight: 500;
        margin: 0;
        text-align: center;

        @media (max-width: 1024px) {
            font-size: 20px;
        }
    }
}
</style>