<template>
    <div class="page">

        <h1>подтверждение покупки</h1>
        <div class="empty" v-if="cart.length <= 0">
            <h2>Здесь пока пусто</h2>

            <div class="links">
                <NuxtLink to="/catalog">Перейти в магазин</NuxtLink>
                <NuxtLink to="/sale">Продать предметы</NuxtLink>
            </div>
        </div>
        <div class="confirm__body" v-else>
            <div class="products">
                <div class="catalog__items">
                    <NuxtLink v-for="item in cart" :key="item.id" :to="'/product/' + item.item.id" class="catalog__item">
                        <span class="disc text-right">- {{ Math.floor(100 - (item.item.sell_price / item.item.price * 100))
                        }}
                            %</span>
                        <div class="img">
                            <img :src="item.item.img" alt="">
                        </div>
                        <h2>{{ item.item.name }}</h2>

                        <div class="price">
                            <small>{{ item.item.price.toFixed(1) }}₸</small>
                            <span>{{ item.item.sell_price.toFixed(1) }}₸</span>
                        </div>
                    </NuxtLink>
                </div>
            </div>

            <div class="confirm__right">
                <span>Количество товаров: {{ cart.length }}</span>
                <span>Итого: {{ totalAmount }} ₸ </span>
                <div class="text-center">
                    <button ref="purchases" @click="confirmPur">Подтвердить покупку</button>
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
            cart: [],
        }
    },
    computed: {
        totalAmount() {
            return this.cart.reduce((total, item) => {
                return total + item.item.sell_price;
            }, 0).toFixed(1).toLocaleString();
        },
    },

    methods: {
        confirmPur() {
            const url = `${this.pathUrl}/api/products/confirm-busket`
            this.$refs.purchases.innerHTML = 'Покупаем'

            const token = this.getAuthorizationCookie();

            axios.defaults.headers.common['Authorization'] = `Token ${token}`;

            axios
                .get(url)
                .then((res) => {
                    console.log(res)
                    this.getCart()
                    this.$refs.purchases.innerHTML = 'Куплено'
                })
                .catch(error => {
                    console.error(error.message);
                    if (error.message == 'Request failed with status code 304') {
                        this.$refs.purchases.innerHTML = 'Недостаточно средств'
                    }
                });
        },
        deleteItem(id) {
            const url = `${this.pathUrl}/api/products/delete-busket-item/${id}`

            const token = this.getAuthorizationCookie();

            axios.defaults.headers.common['Authorization'] = `Token ${token}`;

            axios
                .get(url)
                .then((res) => {
                    this.getCart()
                })
        },
        getCart() {
            const url = `${this.pathUrl}/api/users/profile/`

            const token = this.getAuthorizationCookie();

            axios.defaults.headers.common['Authorization'] = `Token ${token}`;

            axios
                .get(url)
                .then((res) => {
                    this.balance = res.data.balance
                    this.cart = res.data.basket
                })
        },
    },
    mounted() {
        this.getCart()
    },
}
</script>
<script setup>
useSeoMeta({
    title: 'Оформление покупки | RobloMarket',
    ogTitle: 'Оформление покупки | RobloMarket',
    description: 'Оформление покупки | RobloMarket',
    ogDescription: 'Оформление покупки | RobloMarket',
})
</script>
<style lang="scss" scoped>
.page {
    padding: 150px 70px 150px;

    @media (max-width: 1600px) {
        padding: 150px 50px 50px;
    }

    @media (max-width: 1024px) {
        padding: 150px 20px 50px;
    }

    .empty {
        margin-top: 198px;
        display: flex;
        gap: 60px;
        flex-direction: column;
        justify-content: center;
        align-items: center;

        @media (max-width: 1024px) {
            gap: 30px;
            margin-top: 100px;

        }

        h2 {
            font-size: 24px;
            font-style: normal;
            font-weight: 400;
            line-height: 130%;
            /* 31.2px */
            text-transform: uppercase;
            font-family: var(--oxy);
            color: #000;

            @media (max-width: 1024px) {
                font-size: 20px;
            }
        }

        .links {
            display: flex;
            gap: 2.396vw;

            @media (max-width:1024px) {
                gap: 20px;
                width: 100%;
            }
        }

        a {
            border-radius: 10px;
            background: #452777;
            border: 0;
            padding: 12px 20px;
            text-decoration: none;
            transition: all .3s ease;
            text-align: center;

            font-size: 20px;
            font-style: normal;
            font-weight: 400;
            line-height: 130%;
            font-family: var(--oxy);
            color: #fff;

            @media (max-width: 1024px) {
                flex: 1;
                padding: 9.5px 0;
                font-size: 14px;
            }

            &:hover {
                background: #B78BFF;
            }

            &:last-child {
                background: #B78BFF;

                &:hover {
                    background: #452777;
                }
            }
        }
    }

    .confirm__right {
        display: flex;
        flex-direction: column;
        gap: 20px;
        min-width: 20.833vw;
        padding: 25px 25px 30px;
        border-radius: 10px;
        border: 1px solid #A772FF;
        background: #CFB2FF;

        @media (max-width: 1024px) {
            width: 100%;
        }

        span {
            font-size: 20px;
            font-style: normal;
            font-weight: 700;
            line-height: 130%;
            /* 26px */
            text-transform: uppercase;
            font-family: var(--unb);
            color: #fff;

            @media (max-width: 1024px) {
                font-size: 16px;
            }
        }

        button {
            border-radius: 10px;
            background: #452777;
            padding: 12px 20px;
            border: 0;
            transition: all .3s ease;

            font-size: 20px;
            font-style: normal;
            font-weight: 400;
            line-height: 130%;
            font-family: var(--oxy);
            color: #fff;

            @media (max-width: 1024px) {
                padding: 9.5px 20px;
                font-size: 20px;
            }
        }
    }

    .confirm__body {
        display: flex;
        gap: 5.208vw;
        align-items: flex-start;
        margin-top: 40px;

        @media (max-width: 1024px) {
            gap: 30px;
            flex-direction: column;
            margin-top: 20px;
        }

        .products {
            width: 100%;

            .catalog__items {
                display: grid;
                grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
                gap: 20px;
                grid-auto-flow: dense;

                @media (max-width: 1024px) {
                    grid-template-columns: repeat(auto-fill, minmax(165px, 1fr));
                }

                .catalog__item {
                    text-decoration: none;
                    border-radius: 10px;
                    background: #eee;
                    padding: 15px;
                    display: flex;
                    flex-direction: column;
                    gap: 10px;
                    position: relative;

                    @media (max-width: 1024px) {
                        padding: 9px;
                        gap: 5px;
                    }

                    .disc {
                        position: absolute;
                        text-align: right;
                        background: linear-gradient(180deg, #EB05FF 0%, #0031AF 100%);
                        background-clip: text;
                        -webkit-background-clip: text;
                        -webkit-text-fill-color: transparent;
                        font-size: 32px;
                        font-style: normal;
                        font-weight: 500;
                        line-height: 130%;
                        /* 41.6px */
                        text-transform: uppercase;
                        font-family: var(--unb);
                        right: 10%;

                        @media (max-width: 1024px) {
                            font-size: 16px;
                        }
                    }

                    h2 {
                        font-size: 20px;
                        font-style: normal;
                        font-weight: 400;
                        line-height: 130%;
                        text-transform: uppercase;
                        color: #000;
                        font-family: var(--oxy);
                        margin: 0 0 10px;

                        @media (max-width: 1024px) {
                            font-size: 12px;
                        }
                    }

                    .price {
                        margin-top: auto;
                        display: flex;
                        justify-content: flex-end;
                        align-items: flex-start;
                        gap: 8px;

                        span {
                            font-size: 20px;
                            font-style: normal;
                            font-weight: 700;
                            line-height: 130%;
                            /* 26px */
                            text-transform: uppercase;
                            font-family: var(--oxy);
                            color: #000;

                            @media (max-width: 1024px) {
                                font-size: 14px;
                            }
                        }

                        small {
                            font-size: 16px;
                            font-style: normal;
                            font-weight: 400;
                            line-height: 130%;
                            text-decoration: line-through;
                            text-transform: uppercase;
                            font-family: var(--oxy);
                            color: #000;

                            @media (max-width: 1024px) {
                                font-size: 12px;
                            }
                        }
                    }

                    .img {
                        background: #fff;
                        border-radius: 10px;

                        img {
                            width: 100%;
                            height: 250px;
                            object-fit: cover;

                            @media (max-width: 1024px) {
                                height: 147px;
                            }
                        }
                    }
                }
            }
        }
    }

    h1 {
        text-align: center;
        font-size: 40px;
        font-style: normal;
        font-weight: 700;
        line-height: 130%;
        /* 52px */
        letter-spacing: 2px;
        text-transform: uppercase;
        font-family: var(--unb);
        color: #000;
        margin: 0;

        @media (max-width: 1024px) {
            font-size: 20px;
        }
    }
}
</style>