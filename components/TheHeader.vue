<template>
    <header v-if="!hideHeaderOnPages.includes($route.name)">
        <div class="header__pc">
            <div class="links">
                <a href="/#sales">Акции</a>
                <NuxtLink to="/sale">продажа</NuxtLink>
                <NuxtLink to="/catalog">магазин</NuxtLink>
            </div>
            <NuxtLink to="/" class="mr-5">
                <img src="@/assets/img/headerlogo.svg" alt="">
            </NuxtLink>
            <div class="account" v-if="isAuth">
                <NuxtLink to="/withdrawal" class="cash">
                    <span v-if="balance !== null">{{ balance == null ? '0 ₸' :
                        balance.toFixed(1).toLocaleString()
                        + ' ₸' }}</span>
                    <img src="@/assets/img/cash.svg" alt="">
                </NuxtLink>
                <img src="@/assets/img/cart.svg" alt="" style="cursor: pointer;" @click.stop="toggleCart">
                <NuxtLink to="/account">
                    <img src="@/assets/img/account.svg" alt="">
                </NuxtLink>
            </div>
            <NuxtLink to="/register" class="reg" v-else>Вход / Регистрация</NuxtLink>
        </div>
        <div class="header__mob">
            <NuxtLink to="/">
                <img src="@/assets/img/headermob.svg" alt="">
            </NuxtLink>
            <div class="right">
                <img src="@/assets/img/cart.svg" alt="" style="cursor: pointer;" @click.stop="toggleCart">

                <div class="burg">
                    <input id="menu__toggle" class="d-none" type="checkbox" />
                    <label class="menu__btn" for="menu__toggle" @click="menuOpen = !menuOpen">
                        <span></span>
                    </label>
                </div>
            </div>


        </div>
        <div class="mobmenu" :class="{ active: menuOpen }">
            <div class="links">
                <a href="/#sales" @click="menuOpen = !menuOpen">Акции</a>
                <a href="/#about" @click="menuOpen = !menuOpen">продажа</a>
                <NuxtLink to="/catalog" @click="menuOpen = !menuOpen">магазин</NuxtLink>
            </div>
            <div class="links">
                <NuxtLink to="/withdrawal">
                    <span v-if="balance !== null">{{ balance == null ? '0 ₸' :
                        balance.toFixed(1).toLocaleString()
                        + ' ₸' }}</span>
                    <img src="@/assets/img/cash.svg" alt="">
                </NuxtLink>
                <NuxtLink to="/account">
                    <span>Личный кабинет</span>
                    <img src="@/assets/img/account.svg" alt="">
                </NuxtLink>
            </div>
        </div>
    </header>
    <div class="cart" :class="{ cartOpen: cartOpen }" @click="stopPropagation">
        <div class="cart__header">
            <span>Корзина</span>
            <small @click.stop="toggleCart">Закрыть</small>
        </div>

        <div class="cart__body">
            <div class="cart__item" v-for="item in cart" :key="item.id">
                <div class="img">
                    <img :src="item.item.img" alt="">
                </div>
                <div class="item__info">
                    <div class="w-100">
                        <span>{{ item.item.name }}</span>
                        <img src="@/assets/img/delete.svg" alt="" style="cursor: pointer;" @click="deleteItem(item.id)">
                    </div>
                    <small>{{ item.item.sell_price.toFixed(1) }}₸</small>
                </div>
            </div>

        </div>

        <div class="cart__footer">
            <div class="price">
                <span>Итого:</span>
                <span>{{ totalAmount }} ₸ </span>
            </div>
            <NuxtLink to="/cart">Оформить заказ</NuxtLink>
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
            sideActive: false,
            cartOpen: false,
            hideHeaderOnPages: ['login', 'register'],
            menuOpen: false,
            pathUrl: 'https://roblomarket.kz',
            cart: [],
            balance: null,
            isAuth: false,
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
        calculateTotal() {
            let total = 0;

            this.cart.forEach(item => {
                const price = item.products;
                const discountedPrice = price;
                total += discountedPrice * item.amount;
            });

            return total;
        },
        confirmPur() {
            const url = `${this.pathUrl}/api/products/confirm-busket`
            this.$refs.purchase.innerHTML = 'Покупаем'

            const token = this.getAuthorizationCookie();

            axios.defaults.headers.common['Authorization'] = `Token ${token}`;

            axios
                .get(url)
                .then((res) => {
                    console.log(res)
                    this.getCart()
                    this.$refs.purchase.innerHTML = 'Куплено'
                })
                .catch(error => {
                    console.error(error);
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
        toggleCart() {
            this.cartOpen = !this.cartOpen;
            if (this.cartOpen) {
                document.addEventListener('click', this.closeCart);
                this.getCart()
            } else {
                document.removeEventListener('click', this.closeCart);
            }
        },
        closeCart() {
            this.cartOpen = false;
            document.removeEventListener('click', this.closeCart);
        },
        stopPropagation(event) {
            event.stopPropagation();
        },
    },
    mounted() {
        const accType = localStorage.getItem('accountType')

        if (accType == 'steam' || accType == 'email') {
            this.getCart()
            this.isAuth = true
        }
        else {
            this.isAuth = false
        }

    }
}
</script>
<style lang="scss" scoped>
.cartOpen {
    top: 0 !important;
}

.cart {
    position: fixed;
    width: 600px;
    height: 710px;
    padding: 30px 70px 30px 30px;
    border: 1px solid #A772FF;
    background: #CFB2FF;
    z-index: 150;
    right: 0;
    top: -5000px;
    margin-top: 100px;
    transition: all .3s ease;

    @media (max-width: 1024px) {
        width: 100%;
        padding: 20px 20px 39px;
    }

    .cart__footer {
        display: flex;
        flex-direction: column;
        align-items: flex-start;
        gap: 20px;
        margin-top: 20px;

        .price {
            width: 100%;
            display: flex;
            align-items: center;
            justify-content: space-between;

            span {
                font-size: 20px;
                font-style: normal;
                font-weight: 700;
                line-height: 130%;
                /* 26px */
                text-transform: uppercase;
                font-family: var(--unb);
                color: #fff;
            }

        }

        a {
            border-radius: 10px;
            background: #452777;
            padding: 12px 2.552vw;
            display: inline-block;
            text-align: center;
            border: 0;
            transition: all .3s ease;

            font-size: 20px;
            font-style: normal;
            font-weight: 400;
            line-height: 130%;
            font-family: var(--oxy);
            color: #fff;
            text-decoration: none;

            &:hover {
                background: #A772FF;
            }
        }
    }

    .cart__body {
        display: flex;
        flex-direction: column;
        gap: 20px;
        padding: 0 75px 0 0;
        width: 100%;
        height: 470px;
        overflow-x: auto;

        @media (max-width: 1024px) {
            padding: 0;
        }

        .cart__item {
            display: flex;
            gap: 20px;
            align-items: flex-start;
            width: 100%;

            .item__info {
                width: 100%;

                span {
                    font-size: 16px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 130%;
                    /* 20.8px */
                    text-transform: uppercase;
                    font-family: var(--oxy);
                    color: #fff;
                }

                small {
                    font-size: 20px;
                    font-style: normal;
                    font-weight: 700;
                    line-height: 130%;
                    /* 26px */
                    text-transform: uppercase;
                    font-family: var(--oxy);
                    color: #fff;
                }

                div {
                    display: flex;
                    justify-content: space-between;
                    align-items: flex-start;
                    margin: 0 0 15px;

                    img {
                        cursor: pointer;
                    }
                }
            }

            .img {
                border-radius: 10px;
                background: #fff;
                width: 100%;
                max-width: 100px;

                img {
                    width: 100%;
                    height: 100px;
                    object-fit: cover;
                }
            }
        }
    }

    .cart__header {
        margin-bottom: 30px;
        display: flex;
        justify-content: space-between;
        align-items: center;

        span {
            font-size: 20px;
            font-style: normal;
            font-weight: 700;
            line-height: normal;
            text-transform: uppercase;
            font-family: var(--unb);
            color: #fff;
        }

        small {
            font-size: 16px;
            font-style: normal;
            font-weight: 300;
            line-height: normal;
            text-decoration-line: underline;
            font-family: var(--oxy);
            color: #fff;
            cursor: pointer;
        }
    }
}

header {
    position: fixed;
    width: 100%;
    padding: 15px 100px;
    background: #452777;
    z-index: 100;

    @media (max-width: 1600px) {
        padding: 15px 50px;
    }

    @media (max-width: 1024px) {
        padding: 0;
    }

    .active {
        right: 0 !important;
    }

    .mobmenu {
        width: 100%;
        height: 100vh;
        position: absolute;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        background: #452777;
        gap: 55px;
        top: 0;
        right: -5000px;
        transition: all .3s ease;

        .links {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            gap: 40px;

            a {
                font-size: 24px;
                font-style: normal;
                font-weight: 700;
                line-height: normal;
                text-transform: uppercase;
                font-family: var(--oxy);
                color: #fff;

                display: flex;
                align-items: center;
                gap: 20px;
            }
        }
    }

    .header__mob {
        display: none;

        @media (max-width: 1024px) {
            display: flex;
            justify-content: space-between;
            align-items: center;
            width: 100%;
            position: relative;
            padding: 35px 20px 25px 28px;
        }



        .right {
            display: flex;
            gap: 40px;
            align-items: center;
        }
    }

    .header__pc {
        display: flex;
        justify-content: space-between;
        align-items: center;

        @media (max-width: 1024px) {
            display: none;
        }

        .reg {
            font-size: 20px;
            font-style: normal;
            font-weight: 400;
            line-height: normal;
            font-family: var(--oxy);
            color: #fff;
            text-transform: uppercase;
        }

        .account {
            display: flex;
            gap: 40px;

            .cash {
                display: flex;
                align-items: center;
                text-decoration: none;
                gap: 14px;

                span {
                    font-size: 20px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: normal;
                    font-family: var(--oxy);
                    color: #fff;
                }
            }
        }

        .links {
            display: flex;
            gap: 40px;

            a {
                font-size: 20px;
                font-style: normal;
                font-weight: 400;
                line-height: normal;
                text-transform: uppercase;
                font-family: var(--oxy);
                color: #fff;
            }

            img {
                align-self: center;
                /* Добавьте это свойство для логотипа */
            }
        }
    }

}

#menu__toggle {
    opacity: 0;
}

.menu__btn {
    margin-top: 10px;
    display: flex;
    /* используем flex для центрирования содержимого */
    align-items: center;
    /* центрируем содержимое кнопки */
    width: 35px;
    height: 20px;
    cursor: pointer;
    z-index: 100000000;
    color: #fff;
    position: relative;
    transform: rotate(180deg);
}

.menu__btn>span {
    display: block;
    position: absolute;
    width: 100%;
    height: 2px;
    background-color: #fff;
}

.menu__btn>span::before,
.menu__btn>span::after {
    display: block;
    position: absolute;
    width: 100%;
    height: 2px;
    background-color: #fff;
}

.menu__btn>span::before {
    content: '';
    top: -8px;
}

.menu__btn>span::after {
    content: '';
    top: 8px;
}

#menu__toggle:checked~.menu__btn>span {
    transform: rotate(45deg);
    background-color: #fff;
    border-radius: 10px;

}

#menu__toggle:checked~.menu__btn>span::before {
    top: 0;
    transform: rotate(0);
    background-color: #fff;
    border-radius: 10px;

}

#menu__toggle:checked~.menu__btn>span::after {
    top: 0;
    transform: rotate(90deg);
    background-color: #fff;
    border-radius: 10px;
}

#menu__toggle:checked~.menu__box {
    visibility: visible;
    top: 0;
}

.menu__btn>span,
.menu__btn>span::before,
.menu__btn>span::after {
    transition-duration: .25s;
}
</style>