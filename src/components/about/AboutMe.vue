<script>
export default {
	setup() {
		return {
			bios: [
				{
					id: 1,
					bio:
						'Hi, I’m Rogers Mudibo, a seasoned software engineer with over seven years of experience in web and software development. I specialize in building dynamic, user-centric applications and have extensive experience working with technologies such as JavaScript, React, Node.js, TypeScript, Laravel, WordPress, and more.',
				},
				{
					id: 2,
					bio:
						'Outside of work, I’m an avid fan of football , formula one and hiking.',
				},
			],
		};
	},
};
</script>

<template>
	<div class="block sm:flex sm:gap-10 mt-10 sm:mt-20">
		<!-- About profile image -->
		<div class="w-full sm:w-1/4 mb-7 sm:mb-0">
			<img
				src="@/assets/images/rogers.jpeg"
				class="rounded-xl w-96"
				alt=""
			/>
		</div>

		<!-- About details -->
		<div class="w-full sm:w-3/4 text-left">
			<p
				v-for="bio in bios"
				:key="bio.id"
				class="font-general-regular mb-4 text-ternary-dark dark:text-ternary-light text-lg"
			>
				{{ bio.bio }}
			</p>
		</div>
	</div>
</template>
