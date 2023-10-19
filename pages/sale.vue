<template>
    <div class="page">
        <h1>Инвентарь</h1>

        <div class="filters">
            <div class="input-group">
                <div class="input-group-prepend">
                    <span class="input-group-text" id="basic-addon1"><img src="@/assets/img/search.svg" alt=""></span>
                </div>
                <input type="text" :disabled="!sales" class="form-control" placeholder="Поиск по товарам"
                    aria-describedby="basic-addon1" v-model="search" @input="searchProducts">
            </div>
        </div>

        <div class="catalog__items">
            <div class="catalog__item" v-for="item in inventory" :key="item.ied">
                <div class="img">
                    <img :src="item.img" alt="">
                </div>
                <h2>{{ item.name }}</h2>

                <div class="price">
                    <div class="out">
                        <div class="site" @click="openModal(item.id)">
                            <img src="@/assets/img/onsite.svg" alt="">
                            <img src="@/assets/img/banner1.svg" alt="" class="banner1">
                        </div>
                        <div class="outsite" @click="openSteam(item.id)">
                            <img src="@/assets/img/outsite.svg" alt="">
                            <img src="@/assets/img/banner2.svg" alt="" class="banner2">
                        </div>
                    </div>
                    <span>{{ item.sell_price.toFixed(1) }}₸</span>
                </div>
            </div>

        </div>

    </div>
    <ModalFirst :product="modal"></ModalFirst>
    <SteamModal :product="modal"></SteamModal>
</template>
<script>
import global from '~/mixins/global';
import axios from 'axios';
export default {
    mixins: [global],
    data() {
        return {
            sort: false,
            filters: false,
            pathUrl: 'https://roblomarket.kz',
            inventory: [],
            kzt: null,
            usd: null,
            modal: {},
            sales: true,
            search: '',
        }
    },
    methods: {
        openModal(id) {
            this.modal = this.inventory.filter(item => item.id == id)[0];
            $('#modalFirst').modal('show')

        },
        openSteam(id) {
            this.modal = this.inventory.filter(item => item.id == id)[0];
            $('#steamModal').modal('show')
        },


        getTrans() {
            const url = `${this.pathUrl}/api/users/profile/`;
            const token = this.getAuthorizationCookie();
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;

            axios
                .get(url)
                .then((res) => {
                    console.log(res);
                    this.inventory = res.data.transactions_item.map(item => item.item);
                });
        },
        searchProducts() {
            const query = this.search.trim();
            if (query) {
                const queryParams = `?search=${query}`;
                this.fetchSearchResults(queryParams);
            } else {
                this.getInventory();
            }
        },
        fetchSearchResults(queryParams) {
            const path = `${this.pathUrl}/api/products/get-inventory${queryParams}`;
            axios
                .get(path)
                .then(response => {
                    this.inventory = response.data
                })
                .catch(error => {
                    console.error(error);
                });
        },

        toggleFilter() {
            this.filter = !this.filter;
            this.sort = false
            if (this.filter) {
                document.addEventListener('click', this.closeFilter);
            } else {
                document.removeEventListener('click', this.closeFilter);
            }
        },
        closeFilter() {
            this.filter = false;
            document.removeEventListener('click', this.closeFilter);
        },
        toggleSort() {
            this.sort = !this.sort;
            this.filter = false
            if (this.sort) {
                document.addEventListener('click', this.closeSort);
            } else {
                document.removeEventListener('click', this.closeSort);
            }
        },
        closeSort() {
            this.sort = false;
            document.removeEventListener('click', this.closeSort);
        },
        stopPropagation(event) {
            event.stopPropagation();
        },
    },
    mounted() {
        const accType = localStorage.getItem('accountType')

        if (accType != 'email') {
            this.$router.push('/register')
        }
        else {
            this.getTrans()
        }
    }
}
</script>
<script setup>
useSeoMeta({
    title: 'Инвентарь | RobloMarket',
    ogTitle: 'Инвентарь | RobloMarket',
    description: 'Инвентарь | RobloMarket',
    ogDescription: 'Инвентарь | RobloMarket',
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

    .text-right {
        button {
            margin-top: 30px;
            padding: 12px 20px;
            border: 0;
            border-radius: 10px;
            background: #452777;

            font-size: 20px;
            font-style: normal;
            font-weight: 400;
            line-height: 130%;
            font-family: var(--oxy);
            color: #fff;
            transition: all .3s ease;

            &:hover {
                background: #A772FF;
            }

            @media (max-width: 1024px) {
                margin-top: 20px;
                padding: 9.5px 20px;
                font-size: 14px;
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
                justify-content: space-between;
                align-items: center;
                gap: 8px;

                .out {
                    display: flex;
                    gap: 20px;
                    align-items: center;

                    .site:hover .banner1 {
                        transform: scale(1) !important;
                    }

                    .site {
                        cursor: pointer;
                        position: relative;

                        .banner1 {
                            position: absolute;
                            left: -50%;
                            top: -150%;

                            transition: all .3s ease;
                            transform: scale(0);
                        }
                    }

                    .outsite:hover .banner2 {
                        transform: scale(1) !important;
                    }

                    .outsite {
                        cursor: pointer;
                        position: relative;

                        .banner2 {
                            position: absolute;
                            left: -50%;
                            top: -150%;

                            transition: all .3s ease;
                            transform: scale(0);
                        }
                    }
                }

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

    .filters {
        display: flex;
        justify-content: center;
        gap: 30px;
        position: relative;
        align-items: center;
        margin-top: 30px;

        @media (max-width: 1024px) {
            gap: 20px;
            flex-direction: column;
        }

        .activeFil {
            transform: scale(1) !important;
        }

        .sort__body {
            position: absolute;
            right: 22.5%;
            top: 0;
            transform: scale(0);
            transition: all .3s ease;
            margin-top: 70px;
            border-radius: 10px;
            border: 1px solid #A772FF;
            background: #CFB2FF;
            padding: 20px;
            text-align: center;
            z-index: 5;


            @media (max-width: 1024px) {
                width: 100%;
                left: 0;
                margin-top: 125px;
            }

            .activeh {
                border-radius: 5px;
                background: #B78BFF;
            }

            h2 {
                font-size: 16px;
                font-style: normal;
                font-weight: 500;
                line-height: normal;
                text-transform: uppercase;
                font-family: var(--oxy);
                color: #fff;
                margin: 0 0 12px;
                cursor: pointer;
                padding: 6px 12px;
                transition: all .3s ease;

                &:last-child {
                    margin: 0;
                }
            }
        }

        .filter__body {
            position: absolute;
            transform: scale(0);
            transition: all .3s ease;
            right: 22.5%;
            top: 0;
            margin-top: 70px;
            border-radius: 10px;
            border: 1px solid #A772FF;
            background: #CFB2FF;
            padding: 15px 20px;
            display: flex;
            z-index: 5;
            gap: 71px;

            @media (max-width: 1024px) {
                flex-direction: column;
                gap: 10px;
                width: 100%;
                left: 0;
                margin-top: 125px;
            }

            .price {
                display: flex;
                gap: 4px;
                align-items: center;

                input {
                    border-radius: 5px;
                    border: 1px solid #A772FF;
                    background: #fff;
                    padding: 3px 5px;
                    font-size: 16px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: normal;
                    font-family: var(--oxy);
                    color: #000;
                    max-width: 130px;
                    height: 30px;


                    &::placeholder {
                        color: #000;
                    }
                }
            }

            .types {
                .select {
                    margin: 0 0 30px;
                }

                h2 {
                    margin: 0 0 10px !important;
                }

                select {
                    border-radius: 5px;
                    border: 1px solid #0500FF;
                    background: #181831;
                    color: #fff;
                    padding: 5px 10px;

                    font-size: 16px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: normal;
                    font-family: var(--oxy);
                    width: 14.583vw;

                    @media (max-width: 1024px) {
                        width: 100%;
                    }
                }
            }

            h2 {

                font-size: 20px;
                font-style: normal;
                font-weight: 500;
                line-height: normal;
                text-transform: uppercase;
                font-family: var(--oxy);
                margin: 0 0 20px;
                color: #fff;

                &:last-child {
                    margin: 0 0 10px;
                }
            }

            .rare {
                width: 100%;

                .rare__gap {
                    display: grid;
                    gap: 20px;
                    grid-auto-flow: dense;
                    grid-template-columns: repeat(auto-fill, minmax(128px, 1fr));
                }

                label {
                    display: block;
                    margin: 0;
                }

                div {
                    p {
                        font-size: 16px;
                        font-style: normal;
                        font-weight: 500;
                        line-height: normal;
                        font-family: var(--oxy);
                        color: #fff;
                    }
                }
            }

        }

        .input-group {
            max-width: 620px;

            @media (max-width: 1024px) {
                max-width: 100%;
            }
        }

        .input-group-text {
            border-radius: 10px;
            border: 2px solid #A772FF;
            background: #fff;
            border-right: 0;
            border-top-right-radius: 0;
            border-bottom-right-radius: 0;
        }

        input {
            border-radius: 10px;
            border: 2px solid #A772FF;
            background: #fff;
            border-left: 0;
            border-top-left-radius: 0;
            border-bottom-left-radius: 0;
            padding: 10px 20px 10px 0;
            box-shadow: none;

            font-size: 20px;
            font-style: normal;
            font-weight: 300;
            line-height: 32px;
            font-family: var(--oxy);
            color: #000;

            &::placeholder {
                color: #000;
            }
        }

        .buttons {
            display: flex;
            gap: 30px;
            align-items: center;

            @media (max-width: 1024px) {
                gap: 20px;
                width: 100%;
            }

            button {
                border-radius: 10px;
                background: #452777;
                padding: 10px 30px;
                border: 0;

                font-size: 20px;
                font-style: normal;
                font-weight: 400;
                line-height: normal;
                font-family: var(--oxy);
                color: #fff;

                @media (max-width: 1024px) {
                    flex: 1;
                    padding: 10px 0;
                    font-size: 16px;
                }
            }
        }
    }

    h1 {
        font-size: 40px;
        font-style: normal;
        font-weight: 700;
        line-height: 130%;
        letter-spacing: 2px;
        text-transform: uppercase;
        font-family: var(--unb);
        color: #000;
        text-align: center;
        margin: 0;
    }
}

.custom-checkbox p,
.custom-checkbox a {
    @media (max-width: 1024px) {
        font-size: 12px !important;
    }
}

.custom-checkbox input[type="checkbox"] {
    opacity: 0;
    position: absolute;
    border-radius: 3px;
}

.custom-checkbox .checkbox-text:before {
    content: '';
    display: inline-block;
    vertical-align: middle;
    width: 20px;
    height: 20px;
    margin-right: 10px;
    border: 2px solid #fff;
    border-radius: 3px;
    margin-bottom: 3px;
}

.custom-checkbox input[type="checkbox"]:checked+.checkbox-text:before {
    content: url('@/assets/img/check.svg');
    font-size: 10px;
    color: black;
    text-align: center;
    line-height: 20px;
    background: transparent;
}

.custom-checkbox {
    margin-bottom: 20px;

}

.custom-checkbox:last-child {
    margin-bottom: 0 !important;

    @media (max-width: 1024px) {
        margin-bottom: 22px !important;
    }
}
</style>