<template>
    <div>
        <v-app app>
            <v-app-bar app fixed>
                <h1 class="ttl">Let's go to travel</h1>
            </v-app-bar>

            <v-main>
                <v-container style="max-width:500px;">
                    <Nuxt />
                </v-container>
            </v-main>

            <v-bottom-navigation app light fixed color="primary">
                <v-btn v-for="(nav,i) in navs" :key="i" :style="`width:calc(100% / 2); height:100%;background-color:white;`" :to="nav.to" router light>
                    <span>{{ nav.ttl }}</span>
                    <v-icon>{{ nav.icon }}</v-icon>
                </v-btn>
            </v-bottom-navigation>

            <v-btn v-if="scrollY > 300" @click="scrollTop" color="primary" style="bottom:70px;" fixed right fab dark>
                <v-icon dark>mdi-arrow-up</v-icon>
            </v-btn>
        </v-app>
    </div>
</template>

<script>
export default {
    data() {
        return {
            scrollY: 0,
        };
    },
    computed: {
        navs() {
            return [
                {
                    ttl: "ホテル",
                    icon: "mdi-format-list-bulleted",
                    to: "/hotels",
                },
                {
                    ttl: "詳細",
                    icon: "mdi-table",
                    to: "/detail",
                },
            ];
        },
    },
    methods: {
        onScroll() {
            this.scrollY = window.scrollY;
        },
        scrollTop() {
            window.scrollTo({
                top: 0,
                behavior: "smooth",
            });
        },
    },
    mounted() {
        window.addEventListener("scroll", this.onScroll);
    },
};
</script>
<style lang="scss" scoped>
h1 {
    color: #1976d2;
    font-size: 20px;
}
.v-bottom-navigation{
    padding: calc(env(safe-area-inset-bottom)) 0 calc(env(safe-area-inset-bottom) * 1.5);
}
</style>