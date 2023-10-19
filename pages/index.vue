<template>
    <div class="page">
        <div class="main">
            <div class="text-center textik">
                <img src="@/assets/img/mainfuck.png" class="img-fluid" alt="">
                <h1>Создай своего<br> уникального персонажа</h1>
                <div class="links">
                    <NuxtLink to="/catalog">Перейти в магазин</NuxtLink>
                    <NuxtLink to="/sale">Продать предметы</NuxtLink>
                </div>
            </div>
            <div class="marquee_container">
                <div class="marquee_left">
                    <img src="@/assets/img/m1.png" alt="">
                    <img src="@/assets/img/m2.png" alt="">
                    <img src="@/assets/img/m3.png" alt="">
                    <img src="@/assets/img/m4.png" alt="">
                    <img src="@/assets/img/m5.png" alt="">
                    <img src="@/assets/img/m6.png" alt="">
                    <img src="@/assets/img/m7.png" alt="">
                    <img src="@/assets/img/m8.png" alt="">
                    <img src="@/assets/img/m9.png" alt="">
                    <img src="@/assets/img/m11.png" alt="">
                    <img src="@/assets/img/m1.png" alt="">
                    <img src="@/assets/img/m2.png" alt="">
                    <img src="@/assets/img/m3.png" alt="">
                    <img src="@/assets/img/m4.png" alt="">
                    <img src="@/assets/img/m5.png" alt="">
                    <img src="@/assets/img/m6.png" alt="">
                    <img src="@/assets/img/m7.png" alt="">
                    <img src="@/assets/img/m8.png" alt="">
                    <img src="@/assets/img/m9.png" alt="">
                    <img src="@/assets/img/m11.png" alt="">
                    <img src="@/assets/img/m1.png" alt="">
                    <img src="@/assets/img/m2.png" alt="">
                    <img src="@/assets/img/m3.png" alt="">
                    <img src="@/assets/img/m4.png" alt="">
                    <img src="@/assets/img/m5.png" alt="">
                    <img src="@/assets/img/m6.png" alt="">
                    <img src="@/assets/img/m7.png" alt="">
                    <img src="@/assets/img/m8.png" alt="">
                    <img src="@/assets/img/m9.png" alt="">
                    <img src="@/assets/img/m11.png" alt="">
                    <img src="@/assets/img/m1.png" alt="">
                    <img src="@/assets/img/m2.png" alt="">
                    <img src="@/assets/img/m3.png" alt="">
                    <img src="@/assets/img/m4.png" alt="">
                    <img src="@/assets/img/m5.png" alt="">
                    <img src="@/assets/img/m6.png" alt="">
                    <img src="@/assets/img/m7.png" alt="">
                    <img src="@/assets/img/m8.png" alt="">
                    <img src="@/assets/img/m9.png" alt="">
                    <img src="@/assets/img/m11.png" alt="">
                </div>
                <div class="marquee_right">
                    <img src="@/assets/img/m12.png" alt="">
                    <img src="@/assets/img/m13.png" alt="">
                    <img src="@/assets/img/m14.png" alt="">
                    <img src="@/assets/img/m15.png" alt="">
                    <img src="@/assets/img/m16.png" alt="">
                    <img src="@/assets/img/m17.png" alt="">
                    <img src="@/assets/img/m18.png" alt="">
                    <img src="@/assets/img/m19.png" alt="">
                    <img src="@/assets/img/m20.png" alt="">
                    <img src="@/assets/img/m21.png" alt="">
                    <img src="@/assets/img/m22.png" alt="">
                    <img src="@/assets/img/m12.png" alt="">
                    <img src="@/assets/img/m13.png" alt="">
                    <img src="@/assets/img/m14.png" alt="">
                    <img src="@/assets/img/m15.png" alt="">
                    <img src="@/assets/img/m16.png" alt="">
                    <img src="@/assets/img/m17.png" alt="">
                    <img src="@/assets/img/m18.png" alt="">
                    <img src="@/assets/img/m19.png" alt="">
                    <img src="@/assets/img/m20.png" alt="">
                    <img src="@/assets/img/m21.png" alt="">
                    <img src="@/assets/img/m22.png" alt="">
                    <img src="@/assets/img/m12.png" alt="">
                    <img src="@/assets/img/m13.png" alt="">
                    <img src="@/assets/img/m14.png" alt="">
                    <img src="@/assets/img/m15.png" alt="">
                    <img src="@/assets/img/m16.png" alt="">
                    <img src="@/assets/img/m17.png" alt="">
                    <img src="@/assets/img/m18.png" alt="">
                    <img src="@/assets/img/m19.png" alt="">
                    <img src="@/assets/img/m20.png" alt="">
                    <img src="@/assets/img/m21.png" alt="">
                    <img src="@/assets/img/m22.png" alt="">
                    <img src="@/assets/img/m12.png" alt="">
                    <img src="@/assets/img/m13.png" alt="">
                    <img src="@/assets/img/m14.png" alt="">
                    <img src="@/assets/img/m15.png" alt="">
                    <img src="@/assets/img/m16.png" alt="">
                    <img src="@/assets/img/m17.png" alt="">
                    <img src="@/assets/img/m18.png" alt="">
                    <img src="@/assets/img/m19.png" alt="">
                    <img src="@/assets/img/m20.png" alt="">
                    <img src="@/assets/img/m21.png" alt="">
                    <img src="@/assets/img/m22.png" alt="">
                </div>
            </div>
        </div>
        <a name="sales"></a>
        <div class="sales">
            <h1 class="text-center">Акции</h1>
            <div v-if="catalog.length <= 0"></div>
            <div class="catalog__items" v-else>
                <NuxtLink v-for="item in randomItems" :key="item.id" :to="'/product/' + item.id" class="catalog__item">
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

            <div class="text-right">
                <NuxtLink to="/catalog">Посмотреть еще</NuxtLink>
            </div>
        </div>

        <div class="mac text-center">
            <img src="@/assets/img/mac.png" class="macer" alt="">
            <div class="marquee_container">
                <div class="marquee">
                    <h2>АКСЕССУАРЫ</h2>
                    <h2>ПРИЧЕСКИ</h2>
                    <h2>ОДЕЖДА</h2>
                    <h2>ОБУВЬ</h2>
                    <h2>АКСЕССУАРЫ</h2>
                    <h2>ПРИЧЕСКИ</h2>
                    <h2>ОДЕЖДА</h2>
                    <h2>ОБУВЬ</h2>
                    <h2>АКСЕССУАРЫ</h2>
                    <h2>ПРИЧЕСКИ</h2>
                    <h2>ОДЕЖДА</h2>
                    <h2>ОБУВЬ</h2>
                    <h2>АКСЕССУАРЫ</h2>
                    <h2>ПРИЧЕСКИ</h2>
                    <h2>ОДЕЖДА</h2>
                    <h2>ОБУВЬ</h2>
                    <h2>АКСЕССУАРЫ</h2>
                    <h2>ПРИЧЕСКИ</h2>
                    <h2>ОДЕЖДА</h2>
                    <h2>ОБУВЬ</h2>
                    <h2>АКСЕССУАРЫ</h2>
                    <h2>ПРИЧЕСКИ</h2>
                    <h2>ОДЕЖДА</h2>
                    <h2>ОБУВЬ</h2>
                    <h2>АКСЕССУАРЫ</h2>
                    <h2>ПРИЧЕСКИ</h2>
                    <h2>ОДЕЖДА</h2>
                    <h2>ОБУВЬ</h2>
                </div>
            </div>
        </div>

        <div class="sales popular">
            <h1 class="text-center">пОПУЛЯРНОЕ</h1>

            <div v-if="catalog.length <= 0"></div>
            <div class="catalog__items" v-else>
                <NuxtLink v-for="item in randomItemsPop" :key="item.id" :to="'/product/' + item.id" class="catalog__item">
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

            <div class="text-right">
                <NuxtLink to="/catalog">Посмотреть еще</NuxtLink>
            </div>
        </div>
        <a name="about"></a>
        <div class="about">
            <h1 class="text-center">
                о нас
            </h1>
            <div class="about__body">
                <div class="about__left">
                    <div class="img">
                        <img src="@/assets/img/about1.png" alt="">
                        <img src="@/assets/img/about2.png" alt="">
                    </div>
                    <p>Мы разделяем вашу страсть к уникальным скинам и персонализации в игре. Наша цель - сделать ваше
                        путешествие по миру Roblox еще более увлекательным и стильным.</p>
                    <p>Предлагаем широкий выбор скинов, тщательно отбираем самые креативные и качественные. Обеспечиваем
                        безопасность и надежность, гарантируем безопасные транзакции.</p>
                    <p>
                        Дайте вашему персонажу новый облик!</p>

                    <div class="text-right">
                        <NuxtLink to="/register">Регистрация</NuxtLink>
                    </div>
                </div>
                <div class="about__right">
                    <img src="@/assets/img/about3.png" alt="">
                    <img src="@/assets/img/about4.png" alt="">
                </div>
            </div>
        </div>
        <a name="faq"></a>
        <div class="faq">
            <h1 class="text-center">
                Вопросы и ответы
            </h1>

            <div class="faq__body">
                <img src="@/assets/img/faq1.png" alt="" class="img-fluid">
                <div class="faq__left">
                    <button v-for="faq in  faqs " :key="faq.id" :data-target="'#collapseExample' + faq.id"
                        aria-expanded="false" :aria-controls="'collapseExample' + faq.id" @click="toggleCollapse(faq.id)">
                        <div class="inside">
                            <span :style="{ color: faq.isOpen ? '#452777' : '#000' }">
                                {{ faq.question }}
                            </span>
                            <img src="@/assets/img/plus.svg" :class="{ rotate: faq.isOpen }"
                                style="transition: all .3s ease;" alt="">
                        </div>

                        <div :class="{ collapse: !faq.isOpen }" style="transition:  all .3s ease;"
                            :id="'collapseExample' + faq.id">
                            <span class="mt-3 textik">Нет, к сожалению Steam не предоставляет никаких способов для вывода
                                средств с
                                их счета.</span>
                        </div>
                    </button>
                </div>
                <img src="@/assets/img/faq2.png" alt="" class="img-fluid cale">
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
            faqs: [
                { id: 1, isOpen: false, question: '1. Что делать, если после пополнения баланса деньги не поступили на счет?' },
                { id: 2, isOpen: false, question: '2. Что делать, если предмет не пришел или вы не можете его забрать?' },
                { id: 3, isOpen: false, question: '3. Что делать, если к вам в стим добавился “наш сотрудник” и предлагает вам двойной баланс за ваши вещи?' },
                { id: 4, isOpen: false, question: '4. Можно ли перевести средства со счета Steam?' },
                { id: 5, isOpen: false, question: '5. Как продать или вывести предмет с сайта?' },
            ],
            catalog: [],
            pathUrl: 'https://roblomarket.kz',
        }
    },
    computed: {
        randomItems() {
            const items = [...this.catalog.results];

            for (let i = items.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [items[i], items[j]] = [items[j], items[i]];
            }


            return items.slice(0, 6);
        },
        randomItemsPop() {

            const items = [...this.catalog.results];


            for (let i = items.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [items[i], items[j]] = [items[j], items[i]];
            }


            return items.slice(7, 13);
        }
    },
    methods: {
        toggleCollapse(id) {
            const faq = this.faqs.find(f => f.id === id);
            faq.isOpen = !faq.isOpen;
        },
        getCatalog() {
            const url = `${this.pathUrl}/api/products/catalog`

            axios
                .get(url)
                .then(response => {
                    this.catalog = response.data;
                })
                .catch(error => {
                    console.error(error);
                });
        },
    },
    mounted() {
        this.getCatalog()
    }
}
</script>
<script setup>
useSeoMeta({
    title: 'Главная | RobloMarket',
    ogTitle: 'Главная | RobloMarket',
    description: 'Главная | RobloMarket',
    ogDescription: 'Главная | RobloMarket',
})
</script>
<style lang="scss" scoped>
.page {
    background-image: url('@/assets/img/mainbg.png');

    .faq {
        margin: 100px 0 50px;

        @media (max-width: 1400px) {
            padding: 0 50px;
        }

        @media (max-width: 1024px) {
            padding: 0 20px;
            margin: 30px 0 50px;
        }

        .faq__body {
            margin-top: 40px;
            display: flex;
            justify-content: space-evenly;
            align-items: flex-start;

            @media (max-width: 1400px) {
                margin-top: 20px;
                flex-direction: column-reverse;
                width: 100%;
                justify-content: center;
                align-items: center;
            }

            .cale {
                @media (max-width: 1400px) {
                    display: none;
                }
            }

            .faq__left {
                display: flex;
                flex-direction: column;
                gap: 40px;
                // width: 100%;

                @media (max-width: 1400px) {
                    padding: 0 20px;
                    gap: 20px;
                    width: 100%;
                }


                .rotate {
                    transform: rotate(40deg) !important;
                }

                button {
                    width: 100%;
                    max-width: 730px;
                    text-align: left;
                    padding: 0;
                    z-index: 2;
                    background: transparent;
                    border: 0;
                    position: relative;
                    transition: all .3s ease;
                    outline: none;
                    box-shadow: none;

                    @media (max-width: 1400px) {
                        max-width: 100%;
                    }

                    .inside {
                        display: flex;
                        justify-content: space-between;
                        align-items: flex-start;
                        gap: 20px;
                        height: auto;

                    }

                    span {
                        font-size: 20px;
                        font-style: normal;
                        font-weight: 400;
                        line-height: 130%;
                        font-family: var(--oxy);
                        color: #000;
                        max-width: 686px;
                        display: block;
                        // transition: all .3s ease;

                        @media (max-width: 1400px) {
                            font-size: 16px;
                            max-width: 100%;
                        }
                    }

                    .textik {
                        font-size: 16px !important;
                        font-weight: 300 !important;
                    }

                    .collapse {
                        display: none;
                        transition: height 0.3s ease-in-out;
                    }

                    .collapse.show {
                        display: block;
                    }
                }
            }
        }

        h1 {
            font-size: 40px;
            font-style: normal;
            font-weight: 700;
            line-height: 130%;
            text-transform: uppercase;
            font-family: var(--unb);
            color: #A772FF;

            @media (max-width: 1024px) {
                font-size: 20px;
                color: #000;
            }
        }
    }

    .about {
        padding: 0 70px;
        margin-top: 70px;

        @media (max-width: 1600px) {
            padding: 0 50px;

        }

        @media (max-width: 1024px) {
            padding: 0 20px;
            margin-top: 30px;
        }

        img {
            max-width: 100%;

            @media (max-width: 1600px) {
                max-width: 50%;
            }
        }

        .about__body {
            margin-top: 40px;
            display: flex;
            gap: 20px;

            @media (max-width: 1600px) {
                gap: 40px;
            }

            @media (max-width: 1024px) {
                flex-direction: column;
                margin-top: 20px;
                gap: 15px;
            }

            .about__right {
                display: flex;
                gap: 20px;
                align-items: flex-start;

                img {
                    @media (max-width: 1024px) {
                        width: 48%;
                    }
                }
            }

            .about__left {

                p {
                    font-size: 20px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 130%;
                    font-family: var(--oxy);
                    color: #000;
                    max-width: 740px;
                    margin: 0 0 10px;

                    @media (max-width: 1024px) {
                        max-width: 100%;
                        font-size: 16px;
                    }
                }

                a {
                    display: inline-block;
                    text-decoration: none;
                    border-radius: 10px;
                    background: #452777;
                    border: 0;
                    padding: 12px 20px;
                    transition: all .3s ease;

                    font-size: 20px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 130%;
                    font-family: var(--oxy);
                    color: #fff;

                    @media (max-width: 1024px) {
                        width: 100%;
                        text-align: center;
                        font-size: 14px;
                        padding: 9.5px 0;
                    }

                    &:hover {
                        background: #A772FF;
                    }
                }

                .img {
                    display: flex;
                    gap: 20px;
                    margin: 0 0 10px;

                    @media (max-width: 1024px) {
                        width: 100%;
                    }

                    img {
                        @media (max-width: 1024px) {
                            width: 48%;
                        }
                    }
                }
            }
        }

        h1 {
            font-size: 40px;
            font-style: normal;
            font-weight: 700;
            line-height: 130%;
            /* 52px */
            text-transform: uppercase;
            font-family: var(--unb);
            color: #A772FF;

            @media (max-width: 1024px) {
                font-size: 20px;
                color: #000;
            }
        }
    }

    .popular {
        margin-top: 48px !important;

        @media (max-width: 1024px) {
            margin-top: 30px !important;
        }
    }

    .mac {
        margin-top: 50px;
        position: relative;

        @media (max-width: 1024px) {
            margin-top: 30px;
        }

        .macer {
            z-index: 2;
            position: relative;

            max-width: 100%;
        }

        .marquee_container {
            width: 100%;
            overflow: hidden;
            position: absolute;
            top: 50%;
            z-index: 1;

            @media (max-width: 1024px) {
                position: relative;
                top: auto;
                margin-top: 20px;
            }

            .marquee {
                display: flex;
                gap: 70px;
                animation: marquee_mac 15s linear infinite;

                @media (max-width: 1024px) {
                    gap: 20px;
                }

                h2 {
                    font-size: 64px;
                    font-style: normal;
                    font-weight: 600;
                    line-height: 130%;
                    font-family: var(--oxy);
                    color: transparent;
                    -webkit-text-stroke: 2px #A772FF;
                    margin: 0;

                    @media (max-width: 1024px) {
                        font-size: 24px;
                    }
                }
            }

            @keyframes marquee_mac {
                0% {
                    transform: translateX(0);
                }

                100% {
                    transform: translateX(-100%);
                }
            }
        }
    }

    .sales {
        margin-top: 150px;
        padding: 0 70px;

        @media (max-width: 1600px) {
            padding: 0 50px;
        }

        @media (max-width: 1024px) {
            padding: 0 20px;
            margin-top: 30px;
        }

        .text-right {
            a {
                text-decoration: none;
                display: inline-block;
                margin-top: 30px;
                padding: 12px 20px;
                border-radius: 10px;
                background: #A772FF;
                border: 0;
                transition: all .3s ease;

                font-size: 20px;
                font-style: normal;
                font-weight: 400;
                line-height: 130%;
                font-family: var(--oxy);
                color: #fff;

                &:hover {
                    background: #452777;
                }

                @media (max-width: 1024px) {
                    font-size: 14px;
                    padding: 9px 25px;
                    margin-top: 20px;
                }
            }
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

        h1 {
            font-size: 40px;
            font-style: normal;
            font-weight: 700;
            line-height: 130%;
            /* 52px */
            letter-spacing: 2px;
            text-transform: uppercase;
            font-family: var(--unb);
            color: #A772FF;
            margin: 0;

            @media (max-width: 1024px) {
                font-size: 20px;
                color: #000;
            }
        }
    }

    .main {
        margin: 120px 0 0;


        .marquee_container {
            width: 100%;
            overflow: hidden;

            img {
                @media (max-width: 1024px) {
                    width: 70px;
                    height: 70px;
                }
            }
        }

        .marquee_left {
            display: flex;
            gap: 20px;
            margin: 50px 0 20px;
            animation: marquee 15s linear infinite;

            @media (max-width: 1024px) {
                margin: 30px 0 10px;
                gap: 10px;
            }
        }

        .marquee_right {
            display: flex;
            gap: 20px;
            animation: marquee_right 15s linear infinite;

            @media (max-width: 1024px) {
                gap: 10px;
            }
        }

        .marquee-left {
            animation-direction: normal;
        }

        .marquee-right {
            animation-direction: reverse;
        }

        @keyframes marquee {
            0% {
                transform: translateX(0);
            }

            100% {
                transform: translateX(-100%);
            }
        }

        @keyframes marquee_right {
            0% {
                transform: translateX(-100%);
            }

            100% {
                transform: translateX(0);
            }
        }

        .textik {
            @media (max-width: 1024px) {
                padding: 0 20px;
            }

            .links {
                display: flex;
                gap: 46px;
                justify-content: center;

                @media (max-width: 1024px) {
                    gap: 20px;
                }

                a {
                    border-radius: 10px;
                    background: #452777;
                    padding: 12px 20px;
                    border: 0;
                    text-decoration: none;

                    font-size: 20px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 130%;
                    font-family: var(--oxy);
                    color: #fff;
                    transition: all .3s ease;

                    @media (max-width: 1024px) {
                        flex: 1;
                        padding: 12px 0;
                        text-align: center;
                        font-size: 14px;
                    }

                    &:hover {
                        background: #A772FF;
                    }

                    &:last-child {
                        background: #A772FF;

                        &:hover {
                            background: #452777;
                        }
                    }
                }
            }

            h1 {
                font-size: 48px;
                font-style: normal;
                font-weight: 700;
                line-height: 130%;
                text-transform: uppercase;
                font-family: var(--unb);
                color: #000;
                margin: 20px 0 30px;

                @media (max-width: 1024px) {
                    font-size: 24px;
                    margin: 15px 0 30px;
                }
            }
        }
    }
}
</style>