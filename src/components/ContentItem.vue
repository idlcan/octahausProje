<template>
	<div class="max-w-md p-5 bg-white rounded-md">
		<div class="flex flex-row justify-start">
			<div>
				<img
					src="../assets/Ellipse 1@2x.png"
					alt="no image"
					class="w-10 h-10 rounded-full"
				/>
			</div>
			<div class="flex-1 px-2">
				<p class="text-[8px] text-gray-400">
					{{ props.date }} - {{ props.time }}
				</p>
				<p
					v-if="!isEditable"
					class="text-sm"
				>
					<span
						class="font-bold mr-2"
						style="color: #9b59b6"
						>Jane Doe</span
					>{{ props.note }}
				</p>
				<div v-else>
					<span
						class="font-bold mr-2"
						style="color: #9b59b6"
						>Jane Doe</span
					>
					<textarea
						type="text"
						v-model="updatedNote"
						class="w-full resize-none py-2 outline-none"
						:placeholder="props.note"
					/>
					<div class="flex flex-row justify-between items-center pt-3">
						<div class="flex justify-start gap-x-2">
							<AttachIcon />
							<SmileIcon />
						</div>
						<div>
							<button
								class="text-[10px] text-slate-400 py-2 px-4"
								@click="changeEditable"
							>
								CANCEL
							</button>
							<button
								class="text-[10px] py-2 px-4 bg-main text-white rounded-md"
								@click="update"
							>
								UPDATE
							</button>
						</div>
					</div>
				</div>
			</div>
		</div>
		<hr
			class="my-2"
			style="border-color: #eaecee"
		/>
		<div class="flex flex-row justify-between">
			<div class="flex gap-x-2">
				<div class="flex gap-x-2 items-center">
					<LikeIcon
						class="cursor-pointer"
						:class="{ 'text-main': props.like }"
						@click="like(props.id)"
					/>
					<span :class="{ 'text-main': props.like }">{{
						props.like ? '1' : '0'
					}}</span>
				</div>
				<div class="flex gap-x-2 items-center">
					<DislikeIcon
						class="cursor-pointer"
						@click="dislike(props.id)"
					/>
					<span>{{ props.dislike ? '1' : '0' }}</span>
				</div>
			</div>
			<div class="flex gap-x-2">
				<EditIcon
					class="cursor-pointer"
					@click="changeEditable"
				/>
				<DeleteIcon
					class="cursor-pointer"
					@click="deleteItem(props.id)"
				/>
			</div>
		</div>
	</div>
</template>

<script setup>
import DeleteIcon from '../icons/DeleteIcon.vue';
import EditIcon from '../icons/EditIcon.vue';
import LikeIcon from '../icons/LikeIcon.vue';
import DislikeIcon from '../icons/DislikeIcon.vue';

import { ref } from 'vue';
const props = defineProps({
	id: Number,
	note: String,
	date: String,
	time: String,
	like: Boolean,
	dislike: Boolean
});

const emit = defineEmits([
	'delete-item',
	'update-item',
	'like-item',
	'dislike-item'
]);

const updatedNote = ref('');
const isEditable = ref(false);

const deleteItem = (value) => {
	emit('delete-item', value);
};

const update = () => {
	emit('update-item', updatedNote.value, props.id);
	isEditable.value = false;
};

const changeEditable = () => {
	isEditable.value = !isEditable.value;
	updatedNote.value = '';
};

const like = (id) => {
	emit('like-item', id);
};

const dislike = (id) => {
	emit('dislike-item', id);
};
</script>

<style scoped></style>
