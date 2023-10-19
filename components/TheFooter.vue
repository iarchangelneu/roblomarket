<template>
    <footer v-if="!hideHeaderOnPages.includes($route.name)">
        <Nuxtlink to="/">
            <img src="@/assets/img/footerlogo.svg" class="logopype" alt="">
        </Nuxtlink>

        <div>
            <div class="links">
                <div>
                    <a href="/#sales">Акции</a>
                    <a href="/#about">о нас</a>
                </div>
                <div>
                    <NuxtLink to="/catalog">магазин</NuxtLink>
                    <a href="/#faq">вопросы и ответы</a>
                </div>
                <div>
                    <NuxtLink to="/terms">пользовательское соглашение</NuxtLink>
                    <NuxtLink to="/polytics">политика конфиденциальности</NuxtLink>
                </div>
            </div>
            <div class="copyright">
                <small>г. Алматы, пр-кт Республики 13, Почтовый индекс A15X3C5 (050013), +7 775 816 82 48, БИН
                    210840016943<br>
                    Почта поддержки : support@roblox.kz</small>
                <small class="mt-2">2023©roblox.kz </small>
            </div>
        </div>
    </footer>
</template>
<script>
export default {
    data() {
        return {
            hideHeaderOnPages: ['login', 'register'],
        }
    }
}
</script>
<style lang="scss" scoped>
footer {
    width: 100%;
    background: #452777;
    padding: 80px 20px 67px 9.219vw;
    display: flex;
    align-items: flex-start;
    gap: 12.344vw;

    @media (max-width: 1600px) {
        padding: 80px 20px 67px 70px;
        gap: 7.813vw;
    }

    @media (max-width: 1024px) {
        flex-direction: column;
        gap: 30px;

        padding: 45px 20px 35px;
    }

    .logopype {
        @media (max-width: 1024px) {
            max-width: 146px;
        }
    }

    div {

        .copyright {
            margin-top: 30px;

            small {
                font-size: 16px;
                font-style: normal;
                font-weight: 400;
                line-height: 130%;
                font-family: var(--oxy);
                color: #fff;
                display: block;
            }

        }

        .links {
            display: flex;
            gap: 5.208vw;

            @media (max-width: 1600px) {
                gap: 50px;
            }

            @media (max-width: 1024px) {
                gap: 30px;
                flex-direction: column;
            }

            div {
                display: flex;
                flex-direction: column;
                gap: 30px;

                a {
                    font-size: 20px;
                    font-style: normal;
                    font-weight: 700;
                    line-height: normal;
                    text-transform: uppercase;
                    font-family: var(--oxy);
                    color: #fff;

                    @media (max-width: 1400px) {
                        font-size: 16px;
                    }
                }
            }
        }
    }
}
</style>