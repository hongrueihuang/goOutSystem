<script setup>
    import { ref, toRef } from 'vue'
    const titles = ref(['序號', '人員', '外出時間', '返回時間', '事由', '計時', '操作']);
    const props = defineProps({
        allList: Array
    })
    const emits = defineEmits(['backemit'])
    const allList = toRef(props, 'allList');
    const goBack = (list) => {
        if (list.isBack) return;
        const index = allList.value.findIndex(item => item === list);
        const inTime = new Date().getTime();
        allList.value[index].isBack = true;
        allList.value[index].inTime = String(inTime);
        allList.value[index].spendTime = inTime - allList.value[index].outTime;
        emits('backemit', list)
    }
    const formatTime = (time) => {
        const fTime = new Date(+time);
        const yyyy = fTime.getFullYear();
        const mm = addZero(fTime.getMonth() + 1);
        const dd = addZero(fTime.getDate());
        const hh = addZero(fTime.getHours());
        const min = addZero(fTime.getMinutes());
        const ss = addZero(fTime.getSeconds());
        const result = `${yyyy}/${mm}/${dd} ${hh}:${min}:${ss}`;
        return result;
    }
    const addZero = (time) => {
        return time < 10 ? '0' + time : time;
    }
    const countTime = (time) => {
        const allSecond = Math.floor(time / 1000);
        const min = Math.floor(allSecond / 60);
        const ss = allSecond % 60;
        return `${addZero(min)}:${addZero(ss)}`
    }
</script>

<template>
    <div class="h-1/2 overflow-y-auto relative">
        <table class="w-full table-fixed">
            <thead class="sticky top-0">
                <tr>
                    <th
                        class="py-3 bg-slate-600 text-white"
                        v-for="title in titles"
                        :key="title"
                        :width="title === '外出時間' || title === '返回時間' ? '200' : ''">
                        {{ title }}
                    </th>
                </tr>
            </thead>
            <tbody>
                <tr class="bg-white text-black h-20 text-center border-b" v-for="list in allList" :key="`${list.id}-${list.outTime}`">
                    <td>{{ list.id }}</td>
                    <td>{{ list.name }}</td>
                    <td>{{ formatTime(list.outTime) }}</td>
                    <td>{{ list.inTime ? formatTime(list.inTime) : '-' }}</td>
                    <td>{{ list.reason }}</td>
                    <td>{{ list.spendTime ? countTime(list.spendTime) : '-' }}</td>
                    <td @click="goBack(list)">{{ list.isBack ? '-' : '返回' }}</td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<style lang="scss">
    tbody tr:nth-child(2n) {
        @apply bg-gray-200;
    }
</style>