<template>
	<section class="max-w-md mx-auto min-h-screen py-12">
		<div class="flex flex-col gap-y-5">
			<div class="flex- flex-col p-5 rounded-md bg-white">
				<textarea
					v-model="content.note"
					class="w-full resize-none"
					name="item"
					id="item"
					cols="30"
					rows="5"
					placeholder="Your text here"
				></textarea>
				<div class="flex flex-row justify-between items-center pt-3">
					<div class="flex justify-start gap-x-2">
						<AttachIcon />
						<SmileIcon />
					</div>
					<div>
						<button
							class="text-sm text-white rounded-md bg-main py-2 px-5"
							@click="save"
						>
							SUBMIT
						</button>
					</div>
				</div>
			</div>
			<TransitionGroup name="list">
				<div
					v-for="(item, index) in contents"
					:key="index"
				>
					<ContentItem
						:id="item.id"
						:note="item.note"
						:date="item.date"
						:time="item.time"
						:like="item.like"
						:dislike="item.dislike"
						@delete-item="deleteItem"
						@update-item="updateItem"
						@like-item="likeItem"
						@dislike-item="dislikeItem"
					/>
				</div>
			</TransitionGroup>
		</div>
	</section>
</template>

<script setup>
import SmileIcon from '../icons/SmileIcon.vue';
import AttachIcon from '../icons/AttachIcon.vue';
import ContentItem from './ContentItem.vue';
import { ref } from 'vue';

const content = ref({
	id: 0,
	note: '',
	date: '',
	time: '',
	like: false,
	dislike: false
});

const contents = ref([]);
contents.value = JSON.parse(localStorage.getItem('contents')) ?? [];

const save = () => {
	if (!content.value.note) return;
	content.value.date = dateFormatting();
	content.value.time = timeFormatting();
	content.value.id = contents.value.length
		? contents.value[0].id + 1
		: 0;
	contents.value.unshift(content.value);
	localStorage.setItem('contents', JSON.stringify(contents.value));
	content.value = contentReset();
};

const deleteItem = (value) => {
	contents.value = contents.value.filter((item) => item.id !== value);
	localStorage.setItem('contents', JSON.stringify(contents.value));
};

const updateItem = (newNote, id) => {
	if (!newNote) return;
	contents.value.forEach((item) => {
		if (id === item.id) item.note = newNote;
	});
	localStorage.setItem('contents', JSON.stringify(contents.value));
};

const likeItem = (id) => {
	contents.value.forEach((item) => {
		if (id === item.id) {
			item.like = !item.like;
			item.dislike = false;
		}
	});
};

const dislikeItem = (id) => {
	contents.value.forEach((item) => {
		if (id === item.id) {
			item.dislike = !item.dislike;
			item.like = false;
		}
	});
};

const dateFormatting = () => {
	const timeElapsed = Date.now();
	const today = new Date(timeElapsed);
	return `${
		today.getDate() >= 10
			? today.getDate()
			: '0' + String(today.getDate())
	}.${
		today.getMonth() + 1 >= 10
			? today.getMonth() + 1
			: '0' + String(today.getMonth() + 1)
	}.${today.getFullYear()}`;
};

const timeFormatting = () => {
	const timeElapsed = Date.now();
	const today = new Date(timeElapsed);
	return `${
		today.getHours() >= 10
			? today.getHours()
			: '0' + String(today.getHours())
	}:${
		today.getMinutes() >= 10
			? today.getMinutes()
			: '0' + String(today.getMinutes())
	}`;
};

const contentReset = () => {
	return {
		id: 0,
		note: '',
		date: '',
		time: '',
		like: false,
		dislike: false
	};
};
</script>

<style scoped>
.list-enter-active,
.list-leave-active {
	transition: all 0.15s ease;
}
.list-enter-from,
.list-leave-to {
	opacity: 0;
	transform: translateX(30px);
}
</style>
