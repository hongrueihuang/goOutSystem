<script setup>

const emits = defineEmits(['cancelOrder', 'submitOrder']);

const cancel = () => emits('cancelOrder');
const confirm = () => emits('submitOrder');
</script>

<template>
    <div class="absolute top-0 bottom-0 left-0 right-0 bg-gray-700/50 flex items-center justify-center">
        <div class="content w-1/2 rounded-md">
            <header class="py-3 px-2 bg-blue-900 text-white rounded-t-md text-center">確認外出</header>
            <div class="grid grid-cols-2 gap-4 p-10 bg-white rounded-b-md">
                <div class="border rounded-md py-10 text-center" @click="cancel">取消</div>
                <div class="border rounded-md py-10 text-center" @click="confirm">確定</div>
            </div>
        </div>
    </div>
</template>