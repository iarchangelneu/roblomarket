<template>
    <div class="modal fade" id="modalFirst" tabindex="-1" aria-labelledby="modalFirst" aria-hidden="true">
        <div class="modal-dialog modal-dialog-centered">
            <div class="modal-content">

                <div class="modal-body">
                    <h1 class="text-left">Продать</h1>

                    <div class="item__body">
                        <div class="img">
                            <img :src="product.img" alt="">
                        </div>


                        <div>
                            <h2>{{ product.name }}</h2>

                            <span>
                                Ваша цена:
                            </span>

                            <div class="price">
                                <input type="number" v-model="cost">
                                <button @click="saleItem(product.name, product.id)">Продать</button>
                            </div>
                            <small>{{ error }}</small>
                        </div>
                    </div>
                </div>

            </div>
        </div>
    </div>
    <ModalSecond :name="name"></ModalSecond>
</template>
<script>
import global from '~/mixins/global';
import axios from 'axios';
export default {
    mixins: [global],
    props: {
        product: Object,
    },
    data() {
        return {
            cost: null,
            error: '',
            pathUrl: 'https://roblomarket.kz',
            name: '',
        }
    },
    methods: {
        saleItem(name, id) {
            const url = `${this.pathUrl}/api/products/sale-item/${id}/${this.cost}`
            const token = this.getAuthorizationCookie();
            this.name = name
            if (this.cost <= 0) {
                this.error = 'Сумма должна быть > 0'
            }
            else {
                axios.defaults.headers.common['Authorization'] = `Token ${token}`;

                axios
                    .get(url)
                    .then((res) => {
                        console.log(res)
                        if (res.status == 200) {
                            $('#modalFirst').modal('hide')
                            $('#modalSecond').modal('show')
                        }
                    })
                    .catch((error) => {
                        console.log(error)
                    })
            }
        },
    }
}
</script>

<style lang="scss" scoped>
.modal-body {
    padding: 0;

    .item__body {
        display: flex;
        align-items: flex-start;
        gap: 30px;

        @media (max-width: 1024px) {
            flex-direction: column;
            width: 100%;
        }

        .img {
            border-radius: 10px;
            background: var(--White, #FFF);

            img {
                width: 150px;
                height: 150px;
                object-fit: cover;
            }
        }

        small {
            font-family: var(--geo);
            color: red;
            font-size: 14px;
            margin: 10px 0;
        }

        .price {
            display: flex;
            gap: 10px;
            align-items: center;

            input {
                border-radius: 10px;
                border: 2px solid var(--Violet, #A772FF);
                background: var(--Lavanda, #CFB2FF);
                max-width: 164px;
                padding: 9px 7px;

                font-size: 16px;
                font-style: normal;
                font-weight: 400;
                line-height: normal;
                font-family: var(--oxy);
                color: #fff;

                @media (max-width: 1024px) {}
            }

            button {
                border-radius: 10px;
                background: #452777;
                border: 0;
                padding: 10px 3.021vw;
                flex: 1;

                font-size: 20px;
                font-style: normal;
                font-weight: 400;
                line-height: 130%;
                font-family: var(--oxy);
                color: #fff;
            }
        }

        span {
            font-size: 15px;
            font-style: normal;
            font-weight: 300;
            line-height: normal;
            font-family: var(--oxy);
            color: #fff;
            margin: 0 0 10px;
            display: block;
        }

        h2 {
            font-size: 15px;
            font-style: normal;
            font-weight: 300;
            line-height: 130%;
            font-family: var(--oxy);
            color: #fff;
            margin: 0 0 15px;
        }

    }

    h1 {
        font-size: 20px;
        font-style: normal;
        font-weight: 400;
        line-height: 130%;
        /* 31.2px */
        letter-spacing: 1.2px;
        text-transform: uppercase;
        font-family: var(--unb);
        color: #fff;
        margin: 0 0 20px;

        @media (max-width: 1024px) {
            font-size: 16px;
        }
    }
}

.modal-content {
    border-radius: 10px;
    border: 1px solid var(--Violet, #A772FF);
    background: var(--Lavanda, #CFB2FF);
    padding: 20px 20px 50px;
}

@media (min-width: 576px) {
    .modal-dialog {
        max-width: 596px;
        margin: 1.75rem auto;
    }
}

@media (max-width: 1024px) {
    .modal-dialog {
        max-width: 350px;
        margin: 1.75rem auto;
    }
}
</style>