<template>
    <v-list-item @click="isShowHotelDialog = true">
        <v-list-item-avatar>
            <v-img :src="basicInfo.hotelImageUrl"></v-img>
        </v-list-item-avatar>
        <v-list-item-content>
            <v-list-item-title v-html="basicInfo.hotelName"></v-list-item-title>
            <v-list-item-subtitle v-html="basicInfo.access"></v-list-item-subtitle>
        </v-list-item-content>
        <v-dialog v-model="isShowHotelDialog" scrollable>
            <HotelDialog @onCloseMyself="isShowHotelDialog = false" :isShowBtn="true" :basicInfo="basicInfo" :ratingInfo="ratingInfo" />
        </v-dialog>
    </v-list-item>
</template>

<script>
export default {
    props: ["basicInfo", "ratingInfo"],
    data() {
        return {
            isShowHotelDialog: false,
        };
    },
};
</script>

<style>
</style>