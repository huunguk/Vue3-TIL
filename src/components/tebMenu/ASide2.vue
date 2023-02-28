<template>
	<div class="container">
		<h2>Vue Tab Menu</h2>
		<div class="tab-menu">
			<div
				class="menu"
				v-for="(item, index) in list"
				:key="index"
				@click="currentTab = index"
			>
				{{ item.label }}
			</div>
		</div>

		<transition-group name="slide">
			<div
				class="tab-content"
				v-for="(content, index) in list"
				:key="content"
				v-show="currentTab === index"
			>
				{{ content.content }}
			</div>
		</transition-group>
	</div>
</template>

<script setup>
import { ref } from 'vue'

const currentTab = ref(0)
const list = [
	{ label: 'Tab1', content: ' Content1' },
	{ label: 'Tab2', content: ' Content2' },
	{ label: 'Tab3', content: ' Content3' },
	{ label: 'Tab4', content: ' Content4' },
]
</script>

<style scoped>
.transition {
	display: flex;
	background: red;
}
.tab-menu {
	display: flex;
	justify-content: center;
	color: white;
	cursor: pointer;
}

.menu {
	background: navy;
	padding: 10px;
}

.content {
	padding: 10px;
	border: 1px solid black;
}

div {
	width: 300px;
}

.slide-enter-active {
	transition: transform 0.5s ease-in-out;
}

.slide-leave-active {
	transition: transform 0.5s ease-in-out;
}

.slide-enter {
	transform: translateX(100%);
}

.slide-leave-to {
	transform: translateX(-100%);
}
</style>
