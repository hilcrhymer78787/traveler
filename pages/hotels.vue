<template>
    <div>
        <div class="d-flex py-3">
            <v-spacer></v-spacer>
            <v-text-field v-model="keyword" class="rounded-pill" @keydown.enter="searchItemsEnter()" @click:prepend-inner="searchItems()" placeholder="検索" prepend-inner-icon="mdi-magnify" background-color="white" color="teal" outlined dense light clearable hide-details style="width:200px;"></v-text-field>
            <v-spacer></v-spacer>
        </div>
        <div v-if="hotelData">
            <v-pagination class="py-3" color="teal" v-model="hotelData.pagingInfo.page" @input="onClickPagination" :length="hotelData.pagingInfo.pageCount"></v-pagination>
            <v-card>
                <v-card-title>検索結果{{hotelData.pagingInfo.recordCount}}件</v-card-title>
                <v-card-text class="pa-0">
                    <v-list>
                        <HotelItem v-for="(hotelinfo,index) in hotelData.hotels" :key="index" :basicInfo="hotelinfo.hotel[0].hotelBasicInfo" :ratingInfo="hotelinfo.hotel[0].hotelRatingInfo" />
                    </v-list>
                </v-card-text>
            </v-card>
            <v-pagination class="py-3" color="teal" v-model="hotelData.pagingInfo.page" @input="onClickPagination" :length="hotelData.pagingInfo.pageCount"></v-pagination>
        </div>
        <div v-if="hotelErrorData" class="text-center py-5">
            <span class="error_txt">{{errorTxt}}</span>
        </div>
    </div>
</template>

<script>
import axios from "axios";
export default {
    data() {
        return {
            isShowHotelDialog: false,
            getHotelsLoading: false,
            hotelData: null,
            hotelErrorData: null,
            keyword: "東京",
        };
    },
    computed: {
        errorTxt() {
            switch (this.hotelErrorData.data.error) {
                case "not_found":
                    return "検索結果はありませんでした";
                case "wrong_parameter":
                    return "有効なキーワードを指定してください";
                case "too_many_requests":
                    return "リクエストが多いため一時的に通信が制限されました";
                default:
                    return this.hotelErrorData.data.error_description;
            }
        },
    },
    methods: {
        searchItemsEnter() {
            if (event.keyCode !== 13) return;
            document.activeElement.blur();
            this.searchItems();
        },
        searchItems() {
            this.getHotels(1);
        },
        onClickPagination(toPage) {
            this.getHotels(toPage);
        },
        scrollTop() {
            window.scrollTo({ top: 0 });
        },
        async getHotels(toPage) {
            this.scrollTop()
            this.hotelErrorData = null;
            this.getHotelsLoading = true;
            const applicationId = this.$config.appId;
            const keyword = this.keyword;
            const requestConfig = {
                url: `https://app.rakuten.co.jp/services/api/Travel/KeywordHotelSearch/20170426?format=json&keyword=${encodeURIComponent(
                    keyword
                )}&applicationId=${applicationId}&page=${toPage}&hits=10`,
                method: "GET",
            };
            await axios(requestConfig)
                .then((res) => {
                    console.log(res.data);
                    this.hotelData = res.data;
                })
                .catch((err) => {
                    console.log(err.response);
                    this.hotelData = null;
                    this.hotelErrorData = err.response;
                })
                .finally(() => {
                    this.getHotelsLoading = false;
                });
        },
    },
    mounted() {
        this.getHotels(1);
    },
};
</script>

<style lang="scss" scoped>
.error_txt {
    color: red;
}
</style>