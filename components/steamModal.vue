<template>
    <div class="modal fade" id="steamModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog modal-dialog-centered">
            <div class="modal-content">

                <div class="modal-body">
                    <div class="text-center">
                        <h1>Получить предмет</h1>
                        <p class="text-cnter">Инструкция по получению предмета будет отправлена на Вашу почту</p>

                        <button @click="steamOut(product.id)">Подтвердить</button>
                    </div>
                </div>

            </div>
        </div>
    </div>
    <EmailModal></EmailModal>
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
            pathUrl: 'https://roblomarket.kz',
        }
    },
    methods: {
        steamOut(id) {
            const url = `${this.pathUrl}/api/users/send-email`
            const token = this.getAuthorizationCookie();

            axios.defaults.headers.common['Authorization'] = `Token ${token}`;

            axios
                .post(url, {
                    id: id,
                })
                .then((res) => {
                    console.log(res)
                    if (res.status == 200) {
                        $('#steamModal').modal('hide')
                    }
                })
                .catch((error) => {
                    console.log(error)
                    $('#steamModal').modal('hide')
                    $('#emailModal').modal('show')
                })
        },
    }
}
</script>

<style lang="scss" scoped>
.modal-body {
    padding: 0;

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
        margin: 0 0 30px;

        @media (max-width: 1024px) {
            text-align: center;
            font-size: 16px;
            margin: 0 0 20px;
        }
    }

    button {
        border-radius: 10px;
        border: 0;
        background: var(--iris-100, #452777);
        padding: 10px 20px;

        font-size: 20px;
        font-style: normal;
        font-weight: 300;
        line-height: 130%;
        font-family: var(--oxy);
        color: #fff;
    }

    p {
        font-size: 20px;
        font-style: normal;
        font-weight: 300;
        line-height: 130%;
        font-family: var(--oxy);
        color: #fff;
        margin: 0 0 18px;
    }
}

.modal-content {
    border-radius: 10px;
    border: 1px solid var(--Violet, #A772FF);
    background: var(--Lavanda, #CFB2FF);
    padding: 20px 20px 50px;
}

.modal-dialog {
    max-width: 596px;
    margin: 1.75rem auto;
}
</style>