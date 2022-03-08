<template>
    <v-card class="mx-auto">
        <v-card-text class="pa-0">
            <v-img height="200" :src="basicInfo.hotelImageUrl"></v-img>
            <v-simple-table>
                <tbody>
                    <tr>
                        <td>ホテル名</td>
                        <td>{{basicInfo.hotelName}}</td>
                    </tr>
                    <tr>
                        <td>住所</td>
                        <td>{{basicInfo.address1}}{{basicInfo.address2}}</td>
                    </tr>
                    <tr>
                        <td>アクセス</td>
                        <td>{{basicInfo.access}}</td>
                    </tr>
                    <tr>
                        <td>電話番号</td>
                        <td>{{basicInfo.telephoneNo}}</td>
                    </tr>
                    <tr>
                        <td>駐車場</td>
                        <td>{{basicInfo.parkingInformation}}</td>
                    </tr>
                </tbody>
            </v-simple-table>
        </v-card-text>
        <v-divider></v-divider>
        <v-card-actions v-if="isShowBtn">
            <v-spacer></v-spacer>
            <v-btn @click="$emit('onCloseMyself')">
                <v-icon>mdi-close</v-icon>
            </v-btn>
        </v-card-actions>
    </v-card>
</template>

<script>
export default {
    props: ["basicInfo", "ratingInfo", "isShowBtn"],
};
</script>

<style lang="scss" scoped>
tbody {
    tr {
        td {
            &:first-child {
                width: 100px;
            }
            &:last-child {
                width: calc(100% - 100px);
            }
        }
    }
}
</style>