<template>
	<div class="bg-[#FF3179] w-full flex gap-[40px] max-[640px]:gap-5 items-center py-5 min-[920px]:pl-[80px] pl-[32px] max-[640px]:px-4 fixed z-[100]">
		<div class="text-[#FDD602] min-[920px]:text-[32px] text-[24px] max-[640px]:text-[16px] font-Pacifico">Music Market</div>
    <div v-click-outside="handleClickOutSide">
      <input
        type="text"
        id="search" 
        placeholder="Type any name here..."
        class="bg-white rounded-[8px] outline-none py-[18px] pl-[28px] pr-4 w-[361px] relative"
        v-model="searchTerm"
        autocomplete="off"
        @focus="handleFocusInput"
        @keypress.enter.prevent="handleSearch"
      />
      <ul
        class="bg-white rounded-[8px] px-5 absolute top-[85px] max-w-[361px] overflow-y-scroll"
        :class="{'h-[50vh]': listSong.length > 3}"
        v-if="songSuggestions"
      >
        <li
          v-for="(item, index) in listSong"
          :key="index"
          class="flex gap-3 items-center border-b-[1px] border-solid border-[#D9D9D9] pb-5 mt-5 cursor-pointer"
          @click="selectSong(item.nameSong)"
        >
          <img :src="item.avatar" class="max-w-[60px]" alt="">
          <div class="text-[18px] text-[#22313F] font-semibold">{{ item.nameSong }}</div>
        </li>
      </ul>
    </div>
	</div>
</template>
<script lang="ts" setup>
import { ref, watch } from 'vue';

const emits = defineEmits([
  'getListNewSongs'
]);

const searchTerm = ref('')
const songSuggestions = ref(false)

type Songs = {
  nameSong: string,
  singer: string,
  avatar: string
}
const listSong = ref<Songs[]>([])

const getListSuggestionSongs = async () => {
  const res = await fetch('https://mocki.io/v1/785eaca1-7f64-40fc-83f1-e3313de1bf06')
  listSong.value = await res.json()
}
getListSuggestionSongs()

const selectSong = (item: string) => {
  searchTerm.value = item
  emits('getListNewSongs', searchTerm.value)
  songSuggestions.value = false
}

const handleFocusInput = () => {
  songSuggestions.value = true
}

const handleSearch = () => {
  emits('getListNewSongs', searchTerm.value)
  songSuggestions.value = false
}

const handleClickOutSide = () => {
  songSuggestions.value = false
}

watch(searchTerm, () => {
  if (searchTerm.value !== '') {
    let matches = 0

    listSong.value = listSong.value.filter(item => {
      if (
        item.nameSong.toLowerCase().includes(searchTerm.value.toLowerCase())
        && matches < 10
      ) {
        matches++
        return item
      }
    })
  }
  if (searchTerm.value === '') {
    getListSuggestionSongs()
    emits('getListNewSongs', '')
  }
})
</script>
<style>
.v-field {
  background: white;
  border-radius: 8px !important;
  height: 52px;
}
.v-field--center-affix .v-field__append-inner {
  display: none;
}
.v-field--variant-filled .v-field__overlay {
  background-color: unset !important;
}
</style>