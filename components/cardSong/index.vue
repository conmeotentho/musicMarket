<template>
	<div class="flex gap-5">
		<img :src="avatar" class="min-[920px]:max-w-[160px] max-w-[97px] object-cover" alt="" />
		<div>
			<div class="text-[#22313F] min-[920px]:text-[24px] text-[18px] font-semibold min-[920px]:mb-4 mb-3">{{ nameSong }}</div>
			<i class="min-[920px]:text-[20px] text-[16px] text-[#6D6F81]">{{ nameSinger }}</i>
		</div>
	</div>
</template>
<script lang="ts" setup>

const props = defineProps({
	nameSong: String,
	nameSinger: String,
	avatar: String,
});
</script>