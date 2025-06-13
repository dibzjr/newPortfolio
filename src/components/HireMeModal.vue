<script>
import feather from 'feather-icons';
import Button from './reusable/Button.vue';
import FormInput from './reusable/FormInput.vue';
import FormTextarea from './reusable/FormTextarea.vue';

export default {
	props: ['showModal', 'modal', 'categories'],
	components: { Button, FormInput, FormTextarea },
	data() {
		return {
			formData: {
				name: '',
				email: '',
				projectType: '',
				details: ''
			},
			loading: false,
			success: false,
			error: null,
			formErrors: {
				name: '',
				email: '',
				projectType: '',
				details: ''
			}
		};
	},
	methods: {
		validateForm() {
			let isValid = true;
			this.formErrors = {
				name: '',
				email: '',
				projectType: '',
				details: ''
			};

			// Name validation
			if (!this.formData.name.trim()) {
				this.formErrors.name = 'Name is required';
				isValid = false;
			}

			// Email validation
			const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
			if (!this.formData.email.trim()) {
				this.formErrors.email = 'Email is required';
				isValid = false;
			} else if (!emailRegex.test(this.formData.email)) {
				this.formErrors.email = 'Please enter a valid email';
				isValid = false;
			}

			// Project type validation
			if (!this.formData.projectType) {
				this.formErrors.projectType = 'Please select a project type';
				isValid = false;
			}

			// Details validation
			if (!this.formData.details.trim()) {
				this.formErrors.details = 'Please provide project details';
				isValid = false;
			}

			return isValid;
		},
		async handleSubmit(e) {
			e.preventDefault();
			
			if (!this.validateForm()) {
				return;
			}

			this.loading = true;
			this.error = null;
			this.success = false;

			try {
				const response = await fetch('https://formspree.io/f/mqabqnnr', {
					method: 'POST',
					headers: {
						'Content-Type': 'application/json'
					},
					body: JSON.stringify({
						name: this.formData.name,
						email: this.formData.email,
						projectType: this.formData.projectType,
						details: this.formData.details
					})
				});

				if (!response.ok) {
					throw new Error('Failed to send message');
				}

				this.success = true;
				this.formData = {
					name: '',
					email: '',
					projectType: '',
					details: ''
				};
				
				// Close modal after 2 seconds on success
				setTimeout(() => {
					this.showModal();
				}, 2000);
			} catch (error) {
				this.error = 'Failed to send message. Please try again.';
				console.error('Form submission error:', error);
			} finally {
				this.loading = false;
			}
		},
		updateFormData(field, value) {
			this.formData[field] = value;
			// Clear error when user starts typing
			if (this.formErrors[field]) {
				this.formErrors[field] = '';
			}
		}
	},
	mounted() {
		feather.replace();
	},
	updated() {
		feather.replace();
	}
};
</script>

<template>
	<transition name="fade">
		<div v-show="modal" class="font-general-regular fixed inset-0 z-30">
			<!-- Modal body background as backdrop -->
			<div
				v-show="modal"
				@click="showModal()"
				class="bg-filter bg-black bg-opacity-50 fixed inset-0 w-full h-full z-20"
			></div>
			<!-- Modal content -->
			<main
				class="flex flex-col items-center justify-center h-full w-full"
			>
				<transition name="fade-up-down">
					<div
						v-show="modal"
						class="modal-wrapper flex items-center z-30"
					>
						<div
							class="modal max-w-md mx-5 md:max-w-xl bg-secondary-light dark:bg-primary-dark max-h-screen shadow-lg flex-row rounded-lg relative"
						>
							<div
								class="modal-header flex justify-between gap-10 p-5 border-b border-ternary-light dark:border-ternary-dark"
							>
								<h5
									class="text-primary-dark dark:text-primary-light text-xl"
								>
									What project are you looking for?
								</h5>
								<button
									class="px-4 text-primary-dark dark:text-primary-light"
									@click="showModal()"
								>
									<i data-feather="x"></i>
								</button>
							</div>
							<div class="modal-body p-5 w-full h-full">
								<form @submit="handleSubmit" class="max-w-xl m-4 text-left">
									<FormInput
										label="Full Name"
										inputIdentifier="name"
										:val="formData.name"
										@update:val="updateFormData('name', $event)"
										class="mb-2"
									/>
									<div v-if="formErrors.name" class="text-red-500 text-sm mb-4">
										{{ formErrors.name }}
									</div>

									<FormInput
										label="Email"
										inputIdentifier="email"
										inputType="email"
										:val="formData.email"
										@update:val="updateFormData('email', $event)"
									/>
									<div v-if="formErrors.email" class="text-red-500 text-sm mb-4">
										{{ formErrors.email }}
									</div>

									<div class="mt-6 mb-4">
										<label
											class="block mb-2 text-lg text-primary-dark dark:text-primary-light"
											for="project"
											>Project Type</label
										>
										<select
											v-model="formData.projectType"
											@change="updateFormData('projectType', $event.target.value)"
											class="w-full px-5 py-3 border-1 border-gray-200 dark:border-secondary-dark rounded-md text-md bg-secondary-light dark:bg-ternary-dark text-primary-dark dark:text-ternary-light"
											id="project"
											name="project"
											type="text"
											required=""
											aria-label="Project Category"
										>
											<option value="">Select a project type</option>
											<option
												v-for="category in categories"
												:key="category.id"
												:value="category.value"
											>
												{{ category.name }}
											</option>
										</select>
										<div v-if="formErrors.projectType" class="text-red-500 text-sm mt-2">
											{{ formErrors.projectType }}
										</div>
									</div>

									<FormTextarea
										label="Details"
										textareaIdentifier="details"
										:val="formData.details"
										@update:val="updateFormData('details', $event)"
									/>
									<div v-if="formErrors.details" class="text-red-500 text-sm mb-4">
										{{ formErrors.details }}
									</div>

									<!-- Success Message -->
									<div v-if="success" class="text-green-500 mb-4">
										Message sent successfully! We'll get back to you soon.
									</div>

									<!-- Error Message -->
									<div v-if="error" class="text-red-500 mb-4">
										{{ error }}
									</div>

									<div class="mt-7 pb-4 sm:pb-1">
										<Button
											:title="loading ? 'Sending...' : 'Send Request'"
											class="px-4 sm:px-6 py-2 sm:py-2.5 text-white bg-indigo-500 hover:bg-indigo-600 rounded-md focus:ring-1 focus:ring-indigo-900 duration-500"
											type="submit"
											:disabled="loading"
											aria-label="Submit Request"
										/>
									</div>
								</form>
							</div>
							<div
								class="modal-footer mt-2 sm:mt-0 py-5 px-8 border0-t text-right"
							>
								<Button
									title="Close"
									class="px-4 sm:px-6 py-2 bg-gray-600 text-primary-light hover:bg-ternary-dark dark:bg-gray-200 dark:text-secondary-dark dark:hover:bg-primary-light rounded-md focus:ring-1 focus:ring-indigo-900 duration-500"
									@click="showModal()"
									aria-label="Close Modal"
								/>
							</div>
						</div>
					</div>
				</transition>
			</main>
		</div>
	</transition>
</template>

<style scoped>
.modal-body {
	max-height: 570px;
}
.bg-gray-800-opacity {
	background-color: #2d374850;
}
@media screen and (max-width: 768px) {
	.modal-body {
		max-height: 400px;
	}
}
.fade-up-down-enter-active {
	transition: all 0.3s ease;
}
.fade-up-down-leave-active {
	transition: all 0.3s ease;
}
.fade-up-down-enter {
	transform: translateY(10%);
	opacity: 0;
}
.fade-up-down-leave-to {
	transform: translateY(10%);
	opacity: 0;
}

.fade-enter-active {
	-webkit-transition: opacity 2s;
	transition: opacity 0.3s;
}
.fade-leave-active {
	transition: opacity 0.3s;
}

.fade-enter,
.fade-leave-to {
	opacity: 0;
}
</style>
