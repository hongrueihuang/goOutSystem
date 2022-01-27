<script setup>
import { ref, toRef } from 'vue';
import Modal from './Modal.vue';
// 定義props
const props = defineProps({
    members: Array,
    allList: Array
})
// 定義emits
const emits = defineEmits(['submitOrder']);
// 人員列表
const memberList = toRef(props, 'members');
// 全部列表
const allList = toRef(props, 'allList');
// 原因列表
const reasonList = ref(['生病', '突然有事', '生理假', '其他', '天災', '買東西']);
// 假單
const order = ref({});
// 選擇人員
const chooseMember = (person) => {
    if (!person.isBack) return
    order.value = {...person}
}
// 彈窗狀態
const modalShow = ref(false);
// 打開彈窗
const showModal = () => {
    modalShow.value = true;
}
// 送出假單
const submit = () => {
    const outTime = new Date().getTime();
    const orderForm = {
        ...order.value,
        outTime: String(outTime),
        inTime: '',
        spendTime: '',
        isBack: false
    };
    emits('submitOrder', orderForm);
    order.value = {};
    modalShow.value = false;
}
// 驗證假單
const verifyOrder = () => {
    console.log(allList.value, order.value);
}
// 取消假單
const cancelOrder = () => {
    order.value = {};
    modalShow.value = false;
}
</script>

<template>
    <div class="grid grid-cols-4 h-1/2 gap-4 p-4">
        <ul class="member-list">
            <li
                class="member-list-item"
                :class="[{active : member.id === order.id}, {'disabled' : !member.isBack}]"
                v-for="member in members"
                :key="member.name"
                @click="chooseMember(member)">
                {{ member.id }} - {{ member.name }}
            </li>
        </ul>
        <div class="reason">
            <div
                class="reason-option"
                :class="{active : order.reason === reason}"
                v-for="reason in reasonList"
                :key="reason"
                @click="order.reason = reason">
                {{ reason }}
            </div>
        </div>
        <div class="submit">
            <ul class="order-list">
                <li class="order-list-item">人員序號 <b>{{ order.id }}</b></li>
                <li class="order-list-item">人員名稱 <b>{{ order.name }}</b></li>
                <li class="order-list-item">外出理由 <b>{{ order.reason }}</b></li>
            </ul>
            <div class="submit-button w-1/2 ml-auto flex flex-col">
                <button
                    type="button"
                    class="py-3 mb-3 rounded bg-blue-900 text-white"
                    @click="showModal"
                    :disabled="!order.hasOwnProperty('name') || !order.hasOwnProperty('reason')">
                    確定
                </button>
                <button type="button" class="py-3 rounded bg-gray-900 text-white" @click="order = {}">取消</button>
            </div>
        </div>
    </div>
    <Modal v-if="modalShow" :order="order" @submit-order="submit" @cancel-order="cancelOrder"></Modal>
</template>

<style lang="scss">
// 人員列表開始
.member-list {
    @apply border overflow-y-scroll;
    &::-webkit-scrollbar {
        display: none
    }

    .member-list-item {
        @apply border text-center py-5;

        &.active {
            @apply bg-gray-800 text-white;
        }

        &.disabled {
            @apply text-gray-300;
        }
    }
}
// 人員列表結束

// 外出理由開始
.reason {
    @apply p-5 col-span-2 grid grid-cols-2 gap-2 border;

    .reason-option {
        @apply flex items-center justify-center border rounded;

        &.active {
            @apply bg-gray-800 text-white;
        }
    }
}
// 外出理由結束

// 送出取消開始
.submit {
    @apply border flex flex-col justify-between p-3;

    .order-list {
        @apply border rounded h-48 flex flex-col justify-around px-3;
        
        .order-list-item {
            @apply flex justify-between;
        }
    }
}
</style>