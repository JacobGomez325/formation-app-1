<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <div  class="absolute z-30  flex justify-center  h-screen items-center bg-black/[0.78] top-0 right-0   left-0 bottom-0">
		<div class="">
			<div class="bg-white   rounded-md ">
				<div class="flex justify-between items-center px-3 py-1  rounded-t-md bg-gray-400 text-white">
					<h1> {{ props.title }} </h1>
					<span  class="text-red-500 cursor-pointer font-bold text-2xl" @click="$emit('close')">x</span>
				</div>
				<slot></slot>
			</div>
		</div>
  </div>
</template>

<script setup>
	const props = defineProps({
		title: {
			type:String,
			required:true
		}
	})

</script>

<style  scoped>

</style>