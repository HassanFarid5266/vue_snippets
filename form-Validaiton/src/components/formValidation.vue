<template>
    <div class="form">
        <h2>Form Validation (Composition API)</h2>
        <form @submit.prevent="handleSubmit">
            <div class="form-group">
                <label>Name:</label>
                <input v-model="form.name" type="text" />
                <span v-if="errors.name">{{ errors.name }}</span>
            </div>

            <div class="form-group">
                <label>Email:</label>
                <input v-model="form.email" type="email" />
                <span v-if="errors.email">{{ errors.email }}</span>
            </div>

            <div class="form-group">
                <label>Phone:</label>
                <input v-model="form.phone" type="tel" />
                <span v-if="errors.phone">{{ errors.phone }}</span>
            </div>

            <div class="form-group">
                <label>Password:</label>
                <input v-model="form.password" type="password" />
                <span v-if="errors.password">{{ errors.password }}</span>
            </div>

            <div class="form-group">
                <label>Gender:</label>
                <select v-model="form.gender">
                    <option value="">Select</option>
                    <option value="male">Male</option>
                    <option value="female">Female</option>
                </select>
                <span v-if="errors.gender">{{ errors.gender }}</span>
            </div>

            <div class="form-group checkbox-group">
                <label>
                    <input type="checkbox" v-model="form.terms" />
                    Accept Terms
                </label>
                <span v-if="errors.terms">{{ errors.terms }}</span>
            </div>

            <button type="submit">Submit</button>
        </form>

        <pre>{{ form }}</pre>
    </div>
</template>

<script setup>
import { reactive } from 'vue'

const form = reactive({
    name: '',
    email: '',
    phone: '',
    password: '',
    gender: '',
    terms: false,
})

const errors = reactive({})

const handleSubmit = () => {
    Object.keys(errors).forEach(key => delete errors[key])

    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
    const phoneRegex = /^\d{11}$/
    const passwordLength = form.password.length >= 8
    const passwordHasNumber = /\d/.test(form.password)
    const passwordHasSpecial = /[!@#$%^&*]/.test(form.password)

    if (!form.name) errors.name = 'Name is required'
    if (!form.email || !emailRegex.test(form.email)) {
        errors.email = 'Enter a valid email (e.g. user@example.com)'
    }
    if (!form.phone || !phoneRegex.test(form.phone)) {
        errors.phone = 'Phone must be 11 digits'
    }
    if (!form.password || !passwordLength || !passwordHasNumber || !passwordHasSpecial) {
        errors.password =
            'Password must be at least 8 characters, include a number and a special character'
    }
    if (!form.gender) errors.gender = 'Please select your gender'
    if (!form.terms) errors.terms = 'You must accept the terms'

    if (Object.keys(errors).length === 0) {
        alert('Form submitted successfully!')
    }
}
</script>

<style scoped>
input,
select {
    width: 100%;
    padding: 10px 12px;
    border: 1px solid #ccc;
    border-radius: 6px;
    font-size: 14px;
    transition: border 0.2s ease;
}

input:focus,
select:focus {
    outline: none;
    border-color: #5b9bd5;
    box-shadow: 0 0 4px rgba(91, 155, 213, 0.5);
}

.form {
    max-width: 450px;
    margin: 40px auto;
    padding: 24px;
    background: #f9f9f9;
    border-radius: 10px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

h2 {
    text-align: center;
    margin-bottom: 20px;
    color: #333;
}

.form-group {
    margin-bottom: 16px;
}

.form-group input {
    width: 94%;
}

label {
    display: block;
    margin-bottom: 6px;
    font-weight: 500;
    color: #333;
    width: 100%;
}

.checkbox-group label {
    display: flex;
    align-items: center;
    gap: 8px;
    width: 100%;
}

.checkbox-group label input {
    width: auto;
}

span {
    color: #d8000c;
    font-size: 0.85em;
    margin-top: 4px;
    display: block;
}

button {
    width: 100%;
    padding: 12px;
    background-color: #5b9bd5;
    color: white;
    border: none;
    border-radius: 6px;
    font-size: 16px;
    font-weight: bold;
    cursor: pointer;
    transition: background 0.3s ease;
}

button:hover {
    background-color: #417cb8;
}

pre {
    margin-top: 20px;
    background: #f0f0f0;
    padding: 12px;
    border-radius: 6px;
    font-size: 0.85em;
    overflow: auto;
}
</style>