<template>
    <div class="page">
        <prevPage></prevPage>
        <div v-if="product.length <= 0"></div>
        <div class="product" v-else>
            <div class="img">
                <div class="wrapper">
                    <img :src="product.img" alt="">
                </div>
            </div>

            <div class="product__info">
                <h1>{{ product.name }}</h1>

                <span>Категория: {{ product.type }}</span>

                <div v-html="product.descriptions.replace(/\n/g, '<br>')" class="desc"></div>

                <div class="price">
                    <span>{{ product.sell_price.toFixed(1) }}₸</span>
                    <button ref="cart" @click="addCart">Купить</button>
                </div>
            </div>
        </div>

        <div class="similar__product">
            <h2>Похожее</h2>
            <div v-if="catalog.length <= 0"></div>
            <div class="catalog__items" v-else>
                <NuxtLink v-for="item in catalog.results.slice(0, 6)" :key="item.id" :to="'/product/' + item.id"
                    class="catalog__item">
                    <span class="disc text-right">-{{ Math.floor(100 - (item.sell_price / item.price * 100)) }}%</span>
                    <div class="img">
                        <img :src="item.img" alt="">
                    </div>
                    <h2>{{ item.name }}</h2>

                    <div class="price">
                        <small>{{ item.price.toFixed(1) }}₸</small>
                        <span>{{ item.sell_price.toFixed(1) }}₸</span>
                    </div>
                </NuxtLink>
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
            productId: this.$route.params.id,
            product: [],
            catalog: [],
            pathUrl: 'https://roblomarket.kz',
            description: 'Сделайте своего персонажа в мире Roblox еще более уникальным и привлекательным с чарующим рюкзаком. Этот очаровательный аксессуар добавит особый шарм аватару и сделает его неповторимым.'
        }
    },
    methods: {
        addCart() {
            const url = `${this.pathUrl}/api/products/add-busket-item/${this.productId}`
            const token = this.getAuthorizationCookie();

            this.$refs.cart.innerHTML = 'Добавляем'
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            axios
                .get(url)
                .then((res) => {
                    console.log(res)

                    if (res.status == 201) {
                        this.$refs.cart.innerHTML = 'Добавлен'
                    }
                    else (
                        this.$refs.cart.innerHTML = 'Ошибка'
                    )
                })
                .catch((error) => {
                    console.log(error)
                })
        },
        getProduct() {
            const url = `${this.pathUrl}/api/products/get-detail-item/${this.productId}`
            axios
                .get(url)
                .then(response => {
                    this.product = response.data;
                })
                .catch(error => {
                    console.error(error);
                });
        },
        getCatalog() {
            const url = `${this.pathUrl}/api/products/catalog`

            axios
                .get(url)
                .then(response => {
                    this.catalog = response.data;
                    // this.filters = response.data.info_add.filter_type
                })
                .catch(error => {
                    console.error(error);
                });
        },
    },
    mounted() {
        this.getProduct()
        this.getCatalog()
    },
}
</script>
<script setup>
useSeoMeta({
    title: 'Страница товара | RobloMarket',
    ogTitle: 'Страница товара | RobloMarket',
    description: 'Страница товара | RobloMarket',
    ogDescription: 'Страница товара | RobloMarket',
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

    .similar__product {
        margin-top: 117px;

        @media (max-width: 1024px) {
            margin-top: 29px;
        }

        .catalog__items {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
            gap: 20px;
            grid-auto-flow: dense;
            margin-top: 40px;

            @media (max-width: 1024px) {
                grid-template-columns: repeat(auto-fill, minmax(165px, 1fr));
                margin-top: 20px;
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

        h2 {
            font-size: 24px;
            font-style: normal;
            font-weight: 400;
            line-height: 130%;
            /* 31.2px */
            text-transform: uppercase;
            font-family: var(--unb);
            color: #000;

            @media (max-width: 1024px) {
                font-size: 16px;
            }

        }
    }

    .product {
        display: flex;
        align-items: flex-start;
        gap: 30px;
        margin-top: 30px;

        @media (max-width: 1024px) {
            flex-direction: column;
            gap: 20px;
        }

        .product__info {
            h1 {
                font-size: 32px;
                font-style: normal;
                font-weight: 400;
                line-height: 130%;
                /* 41.6px */
                text-transform: uppercase;
                font-family: var(--unb);
                color: #000;
                margin: 0 0 20px;

                @media (max-width: 1024px) {
                    font-size: 20px;
                }
            }

            span {
                font-size: 20px;
                font-style: normal;
                font-weight: 400;
                line-height: 130%;
                /* 26px */
                text-transform: uppercase;
                font-family: var(--oxy);
                color: #000;
                margin: 0 0 20px;
                display: block;

                @media (max-width: 1024px) {
                    font-size: 16px;
                }
            }

            .desc {
                font-size: 20px;
                font-style: normal;
                font-weight: 400;
                line-height: 130%;
                font-family: var(--oxy);
                color: #000;
                max-width: 1098px;

                @media (max-width: 1024px) {
                    font-size: 16px;
                }
            }

            .price {
                display: flex;
                align-items: center;
                gap: 50px;
                margin-top: 70px;

                @media (max-width: 1024px) {
                    margin-top: 30px;
                    gap: 49px;
                }

                span {
                    font-size: 36px;
                    font-style: normal;
                    font-weight: 700;
                    line-height: 130%;
                    /* 46.8px */
                    text-transform: uppercase;
                    font-family: var(--oxy);
                    color: #000;
                    margin: 0;
                }

                button {
                    padding: 12px 65px;
                    border: 0;
                    border-radius: 10px;
                    background: #452777;
                    transition: all .3s ease;

                    font-size: 20px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 130%;
                    font-family: var(--oxy);
                    color: #fff;

                    @media (max-width: 1024px) {
                        padding: 9.5px 58px;
                        font-size: 14px;
                    }

                    &:hover {
                        background: #A772FF;
                    }
                }
            }
        }

        .img {
            border-radius: 10px;
            background: #EEE;
            padding: 15px;

            @media (max-width: 1024px) {
                width: 100%;
            }

            .wrapper {
                border-radius: 10px;
                background: #fff;

                @media (max-width: 1024px) {
                    width: 100%;
                }

                img {
                    width: 100%;
                    height: 250px;
                    object-fit: cover;

                    @media (max-width: 1024px) {
                        height: 312px;
                    }
                }
            }
        }
    }
}
</style>