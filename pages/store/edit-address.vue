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
        editAddress() {
            let obj = {
                id: '1',
                detailAddress: '', //门牌号(详细地址)
                userName: '', //联系人姓名
                userPhone: '', //手机号
                lableName: '', //标签
                sexType: '', //性别
                title: '', //地址概述
                address: '', //详细地址
                longitude: '', //经度
                latitude: '', //纬度
                agreeState: '', //是否是默认地址
            }
            this.$refs.liuChooseAddress.setData(obj)
        }
    }
}

</script>