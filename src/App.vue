<script setup>
import { ref } from 'vue';
import List from './components/List.vue';
import Operater from './components/Operater.vue';
import Modal from './components/Modal.vue';
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup
const memberList = ref([
  {
    id: '01',
    name: 'Luke',
    isBack: true
  },
  {
    id: '02',
    name: 'Riven',
    isBack: true
  },
  {
    id: '03',
    name: 'Kelly',
    isBack: true
  },
  {
    id: '04',
    name: 'Kay',
    isBack: true
  },
  {
    id: '05',
    name: 'Sam',
    isBack: true
  },
  {
    id: '06',
    name: 'David',
    isBack: true
  }
])
const allList = ref([]);
// 送出假單方法
const submitOrder = (item) => {
  const index = memberList.value.findIndex(member => item.name === member.name);
  memberList.value[index].isBack = false;
  allList.value.unshift(item);
}
// 返回簽到方法
const backSign = (item) => {
  const index = memberList.value.findIndex(member => item.name === member.name);
  memberList.value[index].isBack = true;
}
// 確認彈窗
const modalShow = ref(false);
</script>

<template>
  <div class="mx-auto h-full relative">
    <List :all-list="allList" @backemit="backSign"></List>
    <Operater :members="memberList" :all-list="allList" @submit-order="submitOrder"></Operater>
    <Modal v-if="modalShow"></Modal>
  </div>
</template>

<style lang="scss">
  html, body {
    max-width: 1024px;
    height: 768px;
  }

  #app {
    @apply h-full;
  }
</style>
