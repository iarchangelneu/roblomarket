<template>
    <div class="page">
        <div class="form">
            <NuxtLink to="/">
                <img src="@/assets/img/reglogo.svg" alt="">
            </NuxtLink>

            <h1>Регистрация</h1>

            <div class="inputs">
                <div>
                    <label for="email">Ваш e-mail</label>
                    <input type="email" name="email" id="email" v-model="email" placeholder="E-mail">
                </div>
                <div>
                    <label for="password">Пароль</label>
                    <input type="password" name="password" id="password" v-model="password" placeholder="Введите пароль">
                </div>
                <div>
                    <label for="password_repeat">повторите пароль</label>
                    <input type="password" name="password_repeat" id="password_repeat" v-model="password_repeat"
                        placeholder="Повторите пароль">
                </div>
                <small>{{ error }}</small>
                <button @click="register">Регистрация</button>

                <div class="text-center">
                    <span>Есть аккаунт? <NuxtLink to="/login">Войти</NuxtLink></span>
                </div>
            </div>
        </div>
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
            email: '',
            password: '',
            password_repeat: '',
            error: ''
        }
    },
    methods: {
        register() {
            const url = `${this.pathUrl}/api/users/registration`
            const csrf = this.getCSRFToken()

            axios.defaults.headers.common['X-CSRFToken'] = csrf;
            axios
                .post(url, { first_name: this.email, password: this.password, username: this.email, email: this.email })
                .then((res) => {


                    console.log(res)
                    localStorage.setItem('accountType', 'email')
                    if (res.status == 202) {
                        document.cookie = `Authorization=${res.data.token}; expires=Fri, 31 Dec 2023 23:59:59 GMT; path=/`;
                        window.location.href = '/account'
                    }
                    else {
                        this.error = data.username
                    }
                })
                .catch((error) => {
                    this.error = error.response.data.username[0]
                    console.log(error.response);

                });

        },
    },
    mounted() {
        const accType = localStorage.getItem('accountType')

        if (accType == 'steam' || accType == 'email') {
            this.$router.push('/account')

        }
        else {

        }
    }
}
</script>
<script setup>
useSeoMeta({
    title: 'Регистрация | RobloMarket',
    ogTitle: 'Регистрация | RobloMarket',
    description: 'Регистрация | RobloMarket',
    ogDescription: 'Регистрация | RobloMarket',
})
</script>
<style lang="scss" scoped>
.page {
    height: 100vh;
    background-image: url('@/assets/img/reg.png');

    small {
        color: fff;
        font-family: var(--oxy);
        font-size: 14px;
    }

    .form {
        height: 100vh;
        background: rgba(239, 141, 255, 0.30);
        box-shadow: -50px 50px 50px 0px rgba(255, 255, 255, 0.09) inset, 50px -50px 50px 0px rgba(165, 165, 165, 0.09) inset;
        backdrop-filter: blur(15.5px);
        padding: 90px 75px 20px;
        max-width: 600px;
        display: flex;
        flex-direction: column;
        align-items: center;

        span {
            display: block;
            margin-top: 20px;
            font-size: 20px;
            font-style: normal;
            font-weight: 400;
            line-height: normal;
            font-family: var(--oxy);
            color: #fff;

            a {
                color: #fff;
                text-decoration: underline;
            }
        }

        button {
            margin-top: 10px;
            width: 100%;
            border-radius: 10px;
            background: #452777;
            padding: 12px 0;
            border: 0;

            font-size: 20px;
            font-style: normal;
            font-weight: 400;
            line-height: 130%;
            font-family: var(--oxy);
            color: #fff;
        }

        .inputs {
            width: 100%;

            label,
            input {
                display: block;
            }

            label {
                font-size: 24px;
                font-style: normal;
                font-weight: 700;
                line-height: 24px;
                /* 100% */
                text-transform: uppercase;
                font-family: var(--oxy);
                margin: 0 0 10px;
                color: #fff;
            }

            input {
                width: 100%;
                border-radius: 10px;
                border: 2px solid #A772FF;
                background: #CFB2FF;
                padding: 14px 15px;

                font-size: 20px;
                font-style: normal;
                font-weight: 400;
                line-height: 24px;
                font-family: var(--oxy);
                color: #fff;
                margin-bottom: 20px;

                &::placeholder {
                    color: #fff;
                }
            }
        }

        h1 {
            font-size: 40px;
            font-style: normal;
            font-weight: 500;
            line-height: normal;
            font-family: var(--unb);
            color: #fff;
            margin: 84px 0 30px;
        }
    }
}
</style>