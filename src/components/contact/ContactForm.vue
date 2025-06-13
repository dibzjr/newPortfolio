<script>
import Button from '../reusable/Button.vue';
import FormInput from '../reusable/FormInput.vue';
import FormTextarea from '../reusable/FormTextarea.vue';
import emailjs from '@emailjs/browser';

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
			error: null
		};
	},
	methods: {
		async handleSubmit(e) {
			e.preventDefault();
			this.loading = true;
			this.error = null;
			this.success = false;

			try {
				await emailjs.send(
					'YOUR_SERVICE_ID', // Replace with your EmailJS service ID
					'YOUR_TEMPLATE_ID', // Replace with your EmailJS template ID
					{
						from_name: this.formData.name,
						from_email: this.formData.email,
						subject: this.formData.subject,
						message: this.formData.message,
						to_email: 'mudiborogers@gmail.com'
					},
					'YOUR_PUBLIC_KEY' // Replace with your EmailJS public key
				);
				
				this.success = true;
				this.formData = {
					name: '',
					email: '',
					subject: '',
					message: ''
				};
			} catch (error) {
				this.error = 'Failed to send message. Please try again.';
				console.error('EmailJS error:', error);
			} finally {
				this.loading = false;
			}
		},
		updateFormData(field, value) {
			this.formData[field] = value;
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
				<FormInput
					label="Email"
					inputIdentifier="email"
					inputType="email"
					:val="formData.email"
					@update:val="updateFormData('email', $event)"
				/>
				<FormInput 
					label="Subject" 
					inputIdentifier="subject"
					:val="formData.subject"
					@update:val="updateFormData('subject', $event)"
				/>
				<FormTextarea 
					label="Message" 
					textareaIdentifier="message"
					:val="formData.message"
					@update:val="updateFormData('message', $event)"
				/>

				<!-- Success Message -->
				<div v-if="success" class="text-green-500 mb-4">
					Message sent successfully!
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
