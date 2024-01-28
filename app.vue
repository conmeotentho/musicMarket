<template>
  <div>
    <Header @getListNewSongs="getListNewSongs" />
    <div class="bg-white min-[920px]:px-[80px] px-[32px] max-[640px]:px-4 pt-[93px] pb-[96px] max-[640px]:pb-[60px]">
      <el-carousel indicator-position="outside">
        <el-carousel-item>
          <img class="min-[920px]:h-[564px] h-[311px] max-[640px]:h-[152px] object-cover w-full" src="./assets/images/banner_1.png" alt="">
        </el-carousel-item>
        <el-carousel-item>
          <img class="min-[920px]:h-[564px] h-[311px] max-[640px]:h-[152px] object-cover w-full" src="./assets/images/banner_2.png" alt="">
        </el-carousel-item>
        <el-carousel-item>
          <img class="min-[920px]:h-[564px] h-[311px] max-[640px]:h-[152px] object-cover w-full" src="./assets/images/banner_3.png" alt="">
        </el-carousel-item>
      </el-carousel>
      <div class="min-[920px]:mt-[100px] mt-[80px] max-[640px]:mt-[60px]">
        <div class="text-[#22313F] min-[920px]:text-[40px] text-[32px] max-[640px]:text-[24px] font-bold border-b-[1px] border-solid border-[#E1E3E9] pb-3">New Songs</div>
        <div class="min-[920px]:mt-[80px] mt-[60px] max-[640px]:mt-[40px] grid grid-cols-2 max-[640px]:grid-cols-1 min-[920px]:gap-[60px] gap-[40px]">
          <CardSong
            v-for="(item, index) in listNewSongs" :key="index"
            :nameSong="item.nameSong"
            :nameSinger="item.singer"
            :avatar="item.avatar"
          />
        </div>
      </div>
      <div class="min-[920px]:mt-[100px] mt-[80px] max-[640px]:mt-[60px]">
        <div class="text-[#22313F] min-[920px]:text-[40px] text-[32px] max-[640px]:text-[24px] font-bold border-b-[1px] border-solid border-[#E1E3E9] pb-3">Top Download Songs</div>
        <div class="min-[920px]:mt-[80px] mt-[60px] max-[640px]:mt-[40px] grid grid-cols-2 max-[640px]:grid-cols-1 min-[920px]:gap-[60px] gap-[40px]">
          <CardSong
            v-for="(item, index) in listTopSongs" :key="index"
            :nameSong="item.nameSong"
            :nameSinger="item.singer"
            :avatar="item.avatar"
          />
        </div>
      </div>
      <Footer />
    </div>
  </div>
</template>
<script lang="ts" setup>
import { ref } from 'vue';
import Header from './components/header/index.vue';
import Footer from './components/footer/index.vue';
import CardSong from './components/cardSong/index.vue';

type Songs = {
  nameSong: string,
  singer: string,
  avatar: string
}
const listNewSongs = ref<Songs[]>([])
const listTopSongs = ref<Songs[]>([])

const getListNewSongs = async (search: string) => {
  const res = await fetch('https://mocki.io/v1/785eaca1-7f64-40fc-83f1-e3313de1bf06')
  listNewSongs.value = await res.json()
  if(search === '') return
  listNewSongs.value = listNewSongs.value.filter(item => item.nameSong.toLowerCase().includes(search.toLowerCase()))
}
getListNewSongs('')

const getListTopSongs = async () => {
  const res = await fetch('https://mocki.io/v1/3e3a153f-9afe-4af0-8fc4-545e644c5de7')
  listTopSongs.value = await res.json()
}
getListTopSongs()
</script>
<style>
.el-carousel__container {
  height: 564px;
}
@media screen and (max-width: 920px) {
  .el-carousel__container {
    height: 311px;
  }
}
@media screen and (max-width: 640px) {
  .el-carousel__container {
    height: 152px;
  }
}
.el-carousel__indicators--outside {
  display: none;
}
</style>
