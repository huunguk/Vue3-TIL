<template>
	<div class="container">
		<h2>Vue Tab Menu</h2>
		<div class="label">
			<button
				v-for="(item, index) in list"
				:key="index"
				@click="currentTab = index"
				v-bind="item"
			>
				{{ item.label }}
			</button>
		</div>
		<div>
			<transition name="fade">
				<ul class="contents">
					<li
						v-for="(body, index) in list"
						:key="index"
						v-show="currentTab == index"
					>
						{{ body.content }}
					</li>
				</ul>
			</transition>
		</div>
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
li {
	padding: 0px;
	transition: all 0.8s ease;
}

button {
	width: 100px;
	background: navy;
	border: 0;
	padding: 10px 30px;
	color: white;
	cursor: pointer;
	border-bottom: 3px solid yellow;
}
.contents {
	width: 340px;
	list-style: none;
	padding: 30px;
	border: 1px solid black;
}

.content {
	width: 100%;
}
</style>
