<script>
import Button from '../reusable/Button.vue';
import FormInput from '../reusable/FormInput.vue';
import FormTextarea from '../reusable/FormTextarea.vue';

export default {
	components: { Button, FormInput, FormTextarea },
	data() {
		return {
			formData: {
				name: '',
				email: '',
				subject: '',
				message: ''
			},
			loading: false,
			success: false,
			error: null,
			formErrors: {
				name: '',
				email: '',
				subject: '',
				message: ''
			}
		};
	},
	methods: {
		validateForm() {
			let isValid = true;
			this.formErrors = {
				name: '',
				email: '',
				subject: '',
				message: ''
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

			// Subject validation
			if (!this.formData.subject.trim()) {
				this.formErrors.subject = 'Subject is required';
				isValid = false;
			}

			// Message validation
			if (!this.formData.message.trim()) {
				this.formErrors.message = 'Message is required';
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
						subject: this.formData.subject,
						message: this.formData.message
					})
				});

				if (!response.ok) {
					throw new Error('Failed to send message');
				}

				this.success = true;
				this.formData = {
					name: '',
					email: '',
					subject: '',
					message: ''
				};
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
	}
};
</script>

<template>
	<div class="w-full md:w-1/2">
		<div
			class="leading-loose max-w-xl m-4 p-7 bg-secondary-light dark:bg-secondary-dark rounded-xl shadow-xl text-left"
		>
			<p
				class="font-general-medium text-primary-dark dark:text-primary-light text-2xl mb-8"
			>
				Contact Form
			</p>
			<form @submit="handleSubmit" class="font-general-regular space-y-7">
				<FormInput 
					label="Full Name" 
					inputIdentifier="name"
					:val="formData.name"
					@update:val="updateFormData('name', $event)"
				/>
				<div v-if="formErrors.name" class="text-red-500 text-sm -mt-5">
					{{ formErrors.name }}
				</div>

				<FormInput
					label="Email"
					inputIdentifier="email"
					inputType="email"
					:val="formData.email"
					@update:val="updateFormData('email', $event)"
				/>
				<div v-if="formErrors.email" class="text-red-500 text-sm -mt-5">
					{{ formErrors.email }}
				</div>

				<FormInput 
					label="Subject" 
					inputIdentifier="subject"
					:val="formData.subject"
					@update:val="updateFormData('subject', $event)"
				/>
				<div v-if="formErrors.subject" class="text-red-500 text-sm -mt-5">
					{{ formErrors.subject }}
				</div>

				<FormTextarea 
					label="Message" 
					textareaIdentifier="message"
					:val="formData.message"
					@update:val="updateFormData('message', $event)"
				/>
				<div v-if="formErrors.message" class="text-red-500 text-sm -mt-5">
					{{ formErrors.message }}
				</div>

				<!-- Success Message -->
				<div v-if="success" class="text-green-500 mb-4">
					Message sent successfully! We'll get back to you soon.
				</div>

				<!-- Error Message -->
				<div v-if="error" class="text-red-500 mb-4">
					{{ error }}
				</div>

				<div>
					<Button
						:title="loading ? 'Sending...' : 'Send Message'"
						class="px-4 py-2.5 text-white tracking-wider bg-indigo-500 hover:bg-indigo-600 focus:ring-1 focus:ring-indigo-900 rounded-lg duration-500"
						type="submit"
						:disabled="loading"
						aria-label="Send Message"
					/>
				</div>
			</form>
		</div>
	</div>
</template>

<style lang="scss" scoped></style>
