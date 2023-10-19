<template>
    <div class="page">
        <h1>Личный кабинет</h1>

        <div class="navs">
            <span @click="activeTab = 0" :class="{ activeSpan: activeTab == 0 }">Личные данные</span>
            <span @click="activeTab = 1" :class="{ activeSpan: activeTab == 1 }">Транзакции</span>
            <span @click="activeTab = 2" :class="{ activeSpan: activeTab == 2 }">Баланс аккаунта</span>
        </div>

        <div class="account" v-if="activeTab == 0">
            <div class="account__left">
                <h2>{{ userData.username }}</h2>

                <div class="balance">
                    <small>Баланс аккаунта</small>
                    <span v-if="userData.length <= 0"></span>
                    <span v-else>{{ userData.balance.toFixed(1).toLocaleString() + ' ₸' }}</span>

                    <div class="links">
                        <NuxtLink to="/withdrawal">Вывести</NuxtLink>
                        <NuxtLink to="/refill">Пополнить</NuxtLink>
                    </div>
                </div>

                <div class="password">
                    <h2>Пароль</h2>

                    <div>
                        <label for="password">Новый пароль</label>
                        <input type="password" name="password" id="password" v-model="password">
                    </div>
                    <div>
                        <label for="repeat_password">Повторите пароль</label>
                        <input type="password" name="repeat_password" id="repeat_password" v-model="repeat_password">
                    </div>
                    <div class="btn">
                        <button @click="editUser()">Сохранить</button>
                    </div>
                </div>
            </div>

            <div class="account__right">
                <div>
                    <label for="email">E-mail</label>
                    <input type="email" name="email" id="email" v-model="email">
                </div>
                <div class="btn">
                    <button @click="editUser()">Сохранить</button>
                    <button @click="logOut()" class="d-block mt-4">Выйти</button>
                </div>
            </div>
        </div>

        <div class="transactions" v-if="activeTab == 1">
            <!-- <div class="filters">
                <div class="input-group">
                    <div class="input-group-prepend">
                        <span class="input-group-text" id="basic-addon1"><img src="@/assets/img/search.svg" alt=""></span>
                    </div>
                    <input type="text" class="form-control" placeholder="Поиск по товарам" aria-describedby="basic-addon1">
                </div>

                <div class="shit">
                    <div class="selects">
                        <select name="type" id="type">
                            <option value="" disabled selected>Тип операции</option>
                        </select>
                        <select name="type" id="type">
                            <option value="" disabled selected>Статус операции</option>
                        </select>
                    </div>

                </div>
            </div> -->
            <div class="scrollik">
                <!-- <div class="empty">
                    <h1>Список операций будет доступен, когда Вы пополните или выведете средства с баланса</h1>

                    <div class="buttons">
                        <NuxtLink to="/catalog">Магазин</NuxtLink>
                        <NuxtLink to="/sale">Продажа</NuxtLink>
                    </div>
                </div> -->
                <div class="mobtrans">
                    <div class="mobbody">
                        <div class="mob__item" v-for="item in transactions" :key="item">
                            <div class="imageblock">
                                <div class="counts">
                                    <small>Тип</small>
                                    <span>{{ item.type }}</span>
                                </div>
                                <small>Предмет</small>
                                <div class="item text-left">
                                    <img :src="item.item.img" alt="">
                                    <span>{{ item.item.name }}</span>
                                </div>
                            </div>
                            <div class="countsblock">

                                <div class="counts">
                                    <small>Цена</small>
                                    <span>{{ item.item.sell_price.toFixed(1) }} ₸</span>
                                </div>

                                <div class="counts">
                                    <small>Дата</small>
                                    <span>{{ formatDate(item.date) }}</span>
                                </div>
                                <div class="counts">
                                    <small>Статус</small>
                                    <span
                                        :class="{ success: item.status == 'Совершенно', failure: item.status == 'Отменено', process: item.status == 'В процессе' }">{{
                                            item.status }}</span>
                                </div>
                            </div>
                        </div>



                    </div>
                </div>
                <table class="table table-borderless text-center">
                    <thead>
                        <tr>
                            <th scope="col">Тип</th>
                            <th scope="col">Предмет</th>
                            <th scope="col">Цена</th>
                            <th scope="col">Дата</th>
                            <th scope="col">Статус</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="item in transactions" :key="item.id">
                            <td>{{ item.type }}</td>
                            <td class="d-flex justify-content-center">
                                <div class="transitem">
                                    <img :src="item.item.img" alt="">
                                    <span>{{ item.item.name }}</span>

                                </div>
                            </td>
                            <td>{{ item.item.sell_price.toFixed(1) }} ₸</td>
                            <td>{{ formatDate(item.date) }}</td>
                            <td
                                :class="{ success: item.status == 'Совершенно', failure: item.status == 'Отменено', process: item.status == 'В процессе' }">
                                {{ item.status }}</td>
                        </tr>

                    </tbody>
                </table>
            </div>
        </div>

        <div class="balance" v-if="activeTab == 2">

            <div class="scrollik">
                <!-- <div class="empty">
                    <h1>Список операций будет доступен, когда Вы пополните или выведете средства с баланса</h1>
                </div> -->
                <div class="mobtrans">

                    <div class="mob__body" v-for="item in balance.slice().reverse()" :key="item.id">
                        <div class="paddingblya">
                            <div class="body__header">
                                <small>{{ item.type_operation }}</small>
                                <span>{{ formatDate(item.date) }}</span>
                                <img src="@/assets/img/success.svg" v-if="item.paid == 1" alt="">
                                <img src="@/assets/img/failure.svg" v-else alt="">
                            </div>
                            <div class="body__footer">
                                <div>
                                    <small>Цена</small>
                                    <span> {{ item.amount.toLocaleString() }} ₸</span>
                                </div>
                                <div>
                                    <small>Состояние счета</small>
                                    <span>{{ item.amount_now.toFixed(1) }} ₸</span>
                                </div>
                            </div>
                        </div>
                        <hr>
                    </div>
                </div>
                <table class="table table-borderless text-center">
                    <thead>
                        <tr>
                            <th scope="col">Тип</th>
                            <th scope="col">Цена</th>
                            <th scope="col">Дата и время</th>
                            <th scope="col">Статус</th>
                            <th scope="col">Состояние счета</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="item in balance.slice().reverse()" :key="item.data">
                            <td>{{ item.type_operation }}</td>
                            <td>{{ item.amount }} ₸</td>
                            <td>{{ formatDate(item.date) }}</td>
                            <td :class="{ success: item.paid == 1, failure: item.paid == 0 }">{{ item.paid == 1 ?
                                'Совершено'
                                : 'Отменено' }}</td>
                            <td>{{ item.amount_now.toFixed(1) }} ₸</td>
                        </tr>

                    </tbody>
                </table>
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
            activeTab: 0,
            password: '',
            repeat_password: '',
            email: '',
            transactions: [],
            userData: [],
            pathUrl: 'https://roblomarket.kz',
            balance: [],
        }
    },
    methods: {
        formatDate(dateTime) {
            const date = new Date(dateTime);

            const day = String(date.getDate()).padStart(2, '0');
            const month = String(date.getMonth() + 1).padStart(2, '0');
            const year = String(date.getFullYear()).slice(-2);
            const hours = String(date.getHours()).padStart(2, '0');
            const minutes = String(date.getMinutes()).padStart(2, '0');

            return `${day}.${month}.${year} ${hours}:${minutes}`;
        },
        editUser() {
            const url = `${this.pathUrl}/api/users/edit-profile/${this.userData.id}`

            const token = this.getAuthorizationCookie();

            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            if (this.password == '') {
                axios
                    .put(url, {
                        email: this.email,
                        trade_link: this.trade_link,

                    })

                    .then((res) => {
                        console.log(res)

                        this.getUser()
                    })
            }
            else {
                axios
                    .put(url, {
                        email: this.email,
                        password: this.password,
                        trade_link: this.trade_link,

                    })

                    .then((res) => {
                        console.log(res)

                        this.getUser()
                    })
            }
        },
        getUser() {
            const url = `${this.pathUrl}/api/users/profile/`

            const token = this.getAuthorizationCookie();

            axios.defaults.headers.common['Authorization'] = `Token ${token}`;

            axios
                .get(url)
                .then((res) => {
                    console.log(res)
                    this.userData = res.data
                    this.trade_link = res.data.trade_link
                    this.email = res.data.email
                    this.transactions = res.data.transactions_item
                    this.balance = res.data.transactions_balance
                })
        },
    },
    mounted() {
        const accType = localStorage.getItem('accountType')

        if (accType == 'steam' || accType == 'email') {
            this.getUser()

        }
        else {
            this.$router.push('/register')
        }

    }
}
</script>
<script setup>
useSeoMeta({
    title: 'Личный кабинет | RobloMarket',
    ogTitle: 'Личный кабинет | RobloMarket',
    description: 'Личный кабинет | RobloMarket',
    ogDescription: 'Личный кабинет | RobloMarket',
})
</script>
<style lang="scss" scoped>
.page {
    padding: 150px 70px 100px;

    @media (max-width: 1600px) {
        padding: 150px 50px 50px;
    }

    @media (max-width: 1024px) {
        padding: 150px 20px 50px;
    }

    .balance {
        margin-top: 37px;
        // padding: 30px;
        border-radius: 10px;

        hr {
            margin: 20px 0;
            border-top: 1px solid #000;
        }

        .empty {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 700px;

            h1 {
                font-size: 24px;
                font-style: normal;
                font-weight: 400;
                line-height: normal;
                font-family: var(--oxy);
                text-transform: none;
                color: #fff;
            }
        }

        .scrollik {
            height: 700px;
            overflow-y: auto;

        }

        .scrollik {
            scrollbar-color: #999 #333;
        }

        .scrollik::-webkit-scrollbar {
            width: 10px;
            height: 10px;
        }

        .scrollik::-webkit-scrollbar-thumb {
            background: #FF1D00;
            border-radius: 10px;
        }

        .scrollik::-webkit-scrollbar-track {
            background: #000;
            border-radius: 10px;
        }

        .mobtrans {
            display: none;

            // border-radius: 30px;
            // background: #FFF;
            // box-shadow: 0px 0px 15px 0px rgba(47, 59, 163, 0.20);

            .mob__body {

                &:last-child {
                    hr {
                        display: none;
                    }
                }

                .paddingblya {

                    // padding: 30px;


                    .body__header {
                        display: flex;
                        justify-content: space-between;

                        small {
                            font-size: 16px;
                            font-style: normal;
                            font-weight: 500;
                            line-height: normal;
                            font-family: var(--oxy);
                            color: #000;
                        }

                        span {
                            font-size: 16px;
                            font-style: normal;
                            font-weight: 400;
                            line-height: normal;
                            font-family: var(--oxy);
                            color: #000;
                            white-space: nowrap;
                        }
                    }

                    .body__footer {
                        display: flex;
                        justify-content: space-between;
                        margin-top: 15px;
                        text-align: left;

                        small {
                            font-size: 16px;
                            font-style: normal;
                            font-weight: 500;
                            line-height: normal;
                            font-family: var(--oxy);
                            color: #000;
                            text-align: left;
                        }

                        span {
                            font-size: 16px;
                            font-style: normal;
                            font-weight: 400;
                            line-height: normal;
                            font-family: var(--oxy);
                            color: #000;
                            display: block;
                            text-align: left;
                            margin-top: 10px;
                            //   white-space: nowrap;
                        }

                    }
                }
            }

            @media (max-width: 1024px) {
                display: block;
            }
        }

        table {
            @media (max-width: 1024px) {
                display: none;
            }

            th {
                font-size: 20px;
                font-style: normal;
                font-weight: 700;
                line-height: normal;
                font-family: var(--oxy);
                color: #000;
            }

            .success {
                color: #05EA2A !important;
            }

            .failure {
                color: #EA0505 !important;
            }

            .process {
                color: #EAE105 !important;
            }

            .tradelock {
                color: #EF8641 !important;
            }

            td {
                font-size: 24px;
                font-style: normal;
                font-weight: 400;
                line-height: normal;
                font-family: var(--oxy);
                color: #000;

                @media (max-width: 1600px) {
                    font-size: 16px;
                }

                .item {
                    max-width: 130px;
                    padding: 5px;
                    display: flex;
                    flex-direction: column;
                    border-radius: 10px;
                    border: 1.038px solid var(--Rare, #3348E0);
                    background: linear-gradient(90deg, #24212B 0%, rgba(36, 33, 43, 0.65) 100%);

                    span {
                        font-size: 8.306px;
                        font-style: normal;
                        font-weight: 600;
                        line-height: 130%;
                        font-family: var(--oxy);
                        color: #fff;
                        margin: 5px 0 5px;

                        &:last-child {
                            margin: 0;
                        }
                    }

                    img {
                        width: 119px;
                        height: 79px;
                    }
                }
            }
        }
    }

    .transactions {
        padding: 20px 30px;
        border-radius: 10px;
        margin-top: 37px;

        @media (max-width: 1400px) {
            padding: 10px;
        }

        .empty {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 700px;

            h1 {
                font-size: 24px;
                font-style: normal;
                font-weight: 400;
                line-height: normal;
                font-family: var(--oxy);
                text-transform: none;
                color: #fff;
                margin: 0;

                @media (max-width: 1024px) {
                    font-size: 18px;
                }
            }

            .buttons {
                display: flex;
                gap: 30px;
                margin-top: 50px;

                a {
                    border-radius: 10px;
                    border: 0;
                    background: var(--iris-100, #FF1D00);
                    padding: 10px 3.802vw;

                    font-size: 20px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 130%;
                    font-family: var(--oxy);
                    transition: all .3s ease;
                    color: #fff;
                    text-decoration: none;

                    @media (max-width: 1024px) {
                        font-size: 16px;
                    }

                    &:last-child {
                        border: 0;
                        background: #0500FF;


                    }
                }
            }
        }

        .scrollik {
            margin-top: 45px;
            height: 700px;
            overflow-y: auto;

            @media (max-width: 1400px) {
                overflow-y: unset;
                height: auto;
            }
        }

        .scrollik {
            scrollbar-color: #999 #333;
        }

        .scrollik::-webkit-scrollbar {
            width: 10px;
            height: 10px;
        }

        .scrollik::-webkit-scrollbar-thumb {
            background: #452777;
            border-radius: 10px;
        }

        .scrollik::-webkit-scrollbar-track {
            background: #2A1310;
            border-radius: 10px;
        }

        .mobtrans {
            display: none;

            .mobbody {
                display: flex;
                flex-direction: column;
                gap: 40px;



                .mob__item {
                    display: flex;
                    gap: 20px;
                    flex-direction: column;

                    .countsblock {
                        display: flex;
                        flex-direction: column;
                        gap: 20px;
                    }

                    .imageblock {
                        .counts {
                            margin-bottom: 17px;
                        }

                        img {
                            width: 145px;
                            height: 110px;
                        }

                        .item {
                            margin-top: 15px;
                            display: flex;
                            align-items: center;
                        }
                    }

                    small,
                    span {
                        font-size: 16px;
                        font-style: normal;
                        font-weight: 400;
                        line-height: normal;
                        font-family: var(--oxy);
                        color: #000;
                        opacity: 0.5;
                        display: block;
                    }

                    span {
                        color: #000 !important;
                        opacity: 1 !important;
                    }

                    .success {
                        color: #05EA2A !important;
                    }

                    .failure {
                        color: #EA0505 !important;
                    }

                    .process {
                        color: #EAE105 !important;
                    }

                    .tradelock {
                        color: #EF8641 !important;
                    }


                    .counts {
                        display: flex;
                        align-items: center;
                        justify-content: space-between;
                        width: 100%;
                    }
                }
            }

            @media (max-width: 1400px) {
                display: block;
            }
        }

        table {
            @media (max-width: 1400px) {
                display: none;
            }

            th {
                font-size: 20px;
                font-style: normal;
                font-weight: 700;
                line-height: normal;
                font-family: var(--oxy);
                color: #000;
            }

            .success {
                color: #05EA2A !important;
            }

            .failure {
                color: #EA0505 !important;
            }

            .process {
                color: #EAE105 !important;
            }

            .tradelock {
                color: #EF8641 !important;
            }

            td {
                font-size: 20px;
                font-style: normal;
                font-weight: 400;
                line-height: normal;
                font-family: var(--oxy);
                color: #000;

                @media (max-width: 1600px) {
                    font-size: 16px;
                }

                .transitem {
                    display: flex;
                    gap: 10px;
                    align-items: center;
                    width: 50%;

                    img {
                        width: 110px;
                        height: 110px;
                    }

                    span {
                        max-width: 258px;
                    }
                }
            }
        }

        .filters {
            display: flex;
            justify-content: space-between;

            @media (max-width: 1400px) {
                flex-direction: column;
                gap: 15px;
            }

            .shit {
                display: flex;
                gap: 64px;

                @media (max-width: 1400px) {
                    flex-direction: column;
                    gap: 15px;
                }

                .buttons {
                    display: flex;
                    gap: 30px;

                    button {
                        padding: 10px 20px;
                        border-radius: 10px;
                        border: 0;
                        background: var(--iris-100, #FF1D00);

                        font-size: 16px;
                        font-style: normal;
                        font-weight: 400;
                        line-height: 130%;
                        font-family: var(--oxy);
                        color: #fff;
                        transition: all .3s ease;

                        @media (max-width: 1400px) {
                            padding: 10px 0;
                            flex: 1;
                        }

                        &:hover {
                            border: 0;
                            background: #0500FF;
                        }

                        &:last-child {
                            border-radius: 10px;
                            border: 0;
                            background: #0500FF;

                            &:hover {
                                border: 0;
                                background: var(--iris-100, #FF1D00);
                            }
                        }
                    }
                }

                .selects {
                    display: flex;
                    gap: 40px;

                    @media (max-width: 1400px) {
                        justify-content: space-between;
                    }
                }

                select {
                    background: transparent;
                    color: #000;
                    border: 0;

                    font-size: 20px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 32px;
                    font-family: var(--oxy);
                    color: #000;

                    @media (max-width: 1024px) {
                        max-width: 100%;
                        font-size: 12px;
                    }
                }
            }

            input {
                border-radius: 10px;
                border: 2px solid #A772FF;
                background: var(--White, #FFF);
                padding: 10px 20px 10px 0;
                border-left: 0;
                border-top-left-radius: 0;
                border-bottom-left-radius: 0;
                max-width: 590px;

                font-size: 20px;
                font-style: normal;
                font-weight: 400;
                line-height: 32px;
                font-family: var(--oxy);
                color: #000;
                box-shadow: none;

                @media (max-width: 1400px) {
                    max-width: 100%;
                }

                &::placeholder {
                    color: #000;
                }
            }

            .input-group-text {
                border-radius: 10px;
                border: 2px solid #A772FF;
                background: var(--White, #FFF);
                border-right: 0;
                border-top-right-radius: 0;
                border-bottom-right-radius: 0;
            }
        }
    }

    .account {
        display: flex;
        gap: 6.25vw;
        align-items: flex-start;
        margin-top: 41px;
        background-image: url('@/assets/img/accbg.png');
        background-repeat: no-repeat;
        background-position: bottom right;

        @media (max-width: 1024px) {
            flex-direction: column;
            gap: 20px;
            background-size: 100%;
        }

        .btn {
            @media (max-width: 1024px) {
                text-align: right;
                width: 100%;
            }
        }

        button {
            padding: 10px 20px;
            border-radius: 10px;
            background: #452777;
            border: 0;

            font-size: 20px;
            font-style: normal;
            font-weight: 400;
            line-height: 130%;
            font-family: var(--oxy);
            color: #fff;
        }

        .account__right {
            width: 100%;
            margin-top: 30px;

            @media (max-width: 1024px) {
                margin: 0;
            }

            input,
            label {
                display: block;
            }

            label {
                font-size: 20px;
                font-style: normal;
                font-weight: 700;
                line-height: normal;
                font-family: var(--oxy);
                color: #000;
                margin: 0 0 9px;
            }

            input {
                width: 100%;
                max-width: 537px;
                border-radius: 10px;
                border: 2px solid var(--Violet, #A772FF);
                background: var(--Lavanda, #CFB2FF);
                padding: 12px 15px;

                font-size: 16px;
                font-style: normal;
                font-weight: 400;
                line-height: normal;
                font-family: var(--oxy);
                color: #fff;
                margin-bottom: 20px;

                &::placeholder {
                    color: #fff;
                }
            }
        }

        .account__left {
            @media (max-width: 1024px) {
                width: 100%;
            }

            .password {
                margin-top: 30px;

                @media (max-width: 1024px) {
                    margin-top: 20px;
                }

                label,
                input {
                    display: block;
                }

                label {
                    font-size: 16px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: normal;

                    font-family: var(--oxy);
                    color: #000;
                    margin: 0 0 9px;
                }

                input {
                    width: 100%;
                    border-radius: 10px;
                    border: 2px solid var(--Violet, #A772FF);
                    background: var(--Lavanda, #CFB2FF);
                    padding: 12px 15px;

                    font-size: 16px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: normal;
                    font-family: var(--oxy);
                    color: #fff;
                    margin-bottom: 20px;

                    &::placeholder {
                        color: #fff;
                    }
                }

                h2 {
                    font-size: 20px;
                    font-style: normal;
                    font-weight: 700;
                    line-height: normal;
                    font-family: var(--oxy);
                    color: #000;
                    margin: 0 0 20px;

                    @media (max-width: 1024px) {
                        margin: 0 0 10px;
                    }
                }
            }

            .balance {
                border-radius: 10px;
                border: 1px solid var(--Violet, #A772FF);
                background: var(--Lavanda, #CFB2FF);
                padding: 18px 13px 13px;
                width: 450px;

                @media (max-width: 1024px) {
                    width: 100%;
                }

                small,
                span {
                    display: block;
                }

                .links {
                    display: flex;
                    gap: 24px;

                    a {
                        flex: 1;
                        text-align: center;
                        border-radius: 10px;
                        background: #452777;
                        border: 0;
                        padding: 12px 0;

                        font-size: 20px;
                        font-style: normal;
                        font-weight: 400;
                        line-height: 130%;
                        font-family: var(--oxy);
                        color: #fff;

                        @media (max-width: 1024px) {
                            font-size: 14px;
                            padding: 9.5px 0;
                        }
                    }
                }

                small {
                    font-size: 20px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: normal;
                    text-transform: uppercase;
                    font-family: var(--unb);
                    margin: 0 0 13px;
                    color: #fff;
                    text-align: center;

                    @media (max-width: 1024px) {
                        font-size: 16px;
                    }
                }

                span {
                    font-size: 40px;
                    font-style: normal;
                    font-weight: 700;
                    line-height: normal;
                    font-family: var(--oxy);
                    color: #fff;
                    margin: 0 0 30px;
                    text-align: center;

                    @media (max-width: 1024px) {
                        font-size: 32px;
                        margin: 0 0 20px;
                    }
                }
            }

            h2 {
                font-size: 24px;
                font-style: normal;
                font-weight: 700;
                line-height: normal;
                text-transform: uppercase;
                font-family: var(--oxy);
                color: #000;
                margin: 0 0 30px;

                @media (max-width: 1024px) {
                    font-size: 20px;
                    margin: 0 0 20px;
                }
            }
        }
    }

    .navs {
        display: flex;
        justify-content: center;
        align-items: center;
        gap: 50px;
        margin-top: 40px;

        @media (max-width: 1024px) {
            gap: 20px;
            margin-top: 20px;
            justify-content: flex-start;
            flex-wrap: wrap;
        }

        .activeSpan {
            font-weight: 700 !important;
            color: #000 !important;
        }

        span {
            cursor: pointer;
            font-size: 24px;
            font-style: normal;
            font-weight: 300;
            line-height: normal;
            font-family: var(--oxy);
            color: #999;

            @media (max-width: 1024px) {
                font-size: 16px;
            }
        }
    }

    h1 {
        font-size: 40px;
        font-style: normal;
        font-weight: 700;
        line-height: 130%;
        /* 52px */
        letter-spacing: 2px;
        text-transform: uppercase;
        font-family: var(--unb);
        color: #000;
        text-align: center;
        margin: 0;

        @media (max-width: 1024px) {
            font-size: 20px;
        }
    }
}
</style>