<template>
    <div class="page">
        <h1>магазин</h1>

        <div class="filters">
            <div class="input-group">
                <div class="input-group-prepend">
                    <span class="input-group-text" id="basic-addon1"><img src="@/assets/img/search.svg" alt=""></span>
                </div>
                <input type="text" class="form-control" placeholder="Поиск по товарам" aria-label="Поиск по товарам"
                    aria-describedby="basic-addon1" v-model="search" @input="searchProducts">
            </div>

            <div class="buttons">
                <button @click.stop="toggleFilter()">Фильтр</button>
                <button @click.stop="toggleSort()">Сортировка</button>
            </div>
            <div class="sort__body" :class="{ activeFil: sort }">
                <h2 @click="sortActive = 1" :class="{ activeh: sortActive == 1 }">Сначала дешевле</h2>
                <h2 @click="sortActive = 2" :class="{ activeh: sortActive == 2 }">Сначала дороже</h2>
                <h2 @click="sortActive = 2" :class="{ activeh: sortActive == 3 }">Полулярное</h2>
                <h2 @click="sortActive = 2" :class="{ activeh: sortActive == 4 }">Новое</h2>
            </div>
            <div class="filter__body" @click="stopPropagation" :class="{ activeFil: filter }">
                <div class="w-100">
                    <h2>кАТЕГОРИЯ:</h2>
                    <div class="rare">
                        <div class="rare__gap">
                            <label class="custom-checkbox">
                                <input type="checkbox">
                                <p class="checkbox-text m-0">Скаут
                                </p>
                            </label>
                            <label class="custom-checkbox">
                                <input type="checkbox">
                                <p class="checkbox-text m-0">Солдат
                                </p>
                            </label>
                            <label class="custom-checkbox">
                                <input type="checkbox">
                                <p class="checkbox-text m-0">Пиро
                                </p>
                            </label>
                            <label class="custom-checkbox">
                                <input type="checkbox">
                                <p class="checkbox-text m-0">Подрывник
                                </p>
                            </label>
                        </div>

                    </div>
                </div>
                <div class="types">
                    <div>
                        <h2>Цена</h2>
                        <div class="price">
                            <input type="number" id="from" name="from" placeholder="от">
                            <img src="@/assets/img/line.svg" alt="">
                            <input type="number" id="to" name="to" placeholder="до">
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <div class="catalog__items">
            <NuxtLink v-for="item in catalog.results" :key="item.id" :to="'/product/' + item.id" class="catalog__item">
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
            <button ref="showmore" @click="loadMoreProducts()">Показать еще</button>
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
            filter: false,
            sort: false,
            sortActive: 0,
            catalog: [],
            pathUrl: 'https://roblomarket.kz',
            search: '',
        }
    },
    methods: {
        loadMoreProducts() {
            if (this.catalog.links.next) {
                this.$refs.showmore.innerHTML = 'Загружаем'
                axios
                    .get(this.catalog.links.next)
                    .then(response => {
                        this.$refs.showmore.innerHTML = 'Показать еще'
                        this.catalog.results.push(...response.data.results);
                        this.catalog.links.next = response.data.links.next;
                    })
                    .catch(error => {
                        console.error(error);
                    });
            }
            else {
                this.$refs.showmore.innerHTML = 'Больше ничего нет (;'
            }
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
        searchProducts() {
            const query = this.search.trim();
            if (query) {
                const queryParams = `?search=${query}`;
                this.fetchSearchResults(queryParams);
            } else {
                this.getCatalog();
            }
        },
        fetchSearchResults(queryParams) {
            const path = `${this.pathUrl}/api/products/catalog${queryParams}`;
            axios
                .get(path)
                .then(response => {
                    this.catalog = response.data;
                    this.filters = response.data.info_add.filter_type
                })
                .catch(error => {
                    console.error(error);
                });
        },
        applyFilters() {
            let queryParams = '';
            if (this.selectedRarity) {
                queryParams += `&tags__редкость=${this.selectedRarity}`;
            }
            if (this.selectedHero) {
                queryParams += `&tags__герой=${this.selectedHero}`;
            }
            if (this.selectedCell) {
                queryParams += `&tags__ячейка=${this.selectedCell}`;
            }
            if (this.minPrice !== null) {
                queryParams += `&min_price=${this.minPrice}`;
            }
            if (this.maxPrice !== null) {
                queryParams += `&max_price=${this.maxPrice}`;
            }

            // Remove the leading '&' if there are any query parameters
            if (queryParams) {
                queryParams = `?${queryParams.substring(1)}`;
            }

            const path = `${this.pathUrl}/api/products/catalog${queryParams}`;
            axios
                .get(path)
                .then((response) => {
                    this.catalog = response.data;
                    this.filters = response.data.info_add.filter_type
                })
                .catch((error) => {
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
        this.getCatalog()
    }
}
</script>
<script setup>
useSeoMeta({
    title: 'Магазин | RobloMarket',
    ogTitle: 'Магазин | RobloMarket',
    description: 'Магазин | RobloMarket',
    ogDescription: 'Магазин | RobloMarket',
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