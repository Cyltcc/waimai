<template>
    <view class="page-main">
        <liu-chooseAddress ref="liuChooseAddress" @submit="submitAddress" @detele="deteleAddress"></liu-chooseAddress>
    </view>
</template>

<script>

export default {
    data() {
        return {

        }
    },
    methods: {
        submitAddress(e) {
                console.log('添加调地址信息：', e)
            },
            //删除地址回调
            deteleAddress(e) {
                console.log('所删除的地址信息：', e)
            },
    }
}

</script>