<!-- Login.svelte -->
<script>
	import { createEventDispatcher } from 'svelte';

    import {goto} from '$app/navigation'
	// import { useState } from 'svelte/internal';

	const dispatcher = createEventDispatcher();
	const userTypes = ['Admin', 'User', 'Dealership'];
	let userType = '';
	let formData = {};

	const handleSubmit = async () => {
		console.log(formData);
		try {
            const response = await fetch(`http://localhost:5001/api/${userType.toLowerCase()}/login`, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify(formData)
            });

            const data = await response.json();
            console.log(data);

            if (response.ok) {
                localStorage.setItem("user_type",userType)
                localStorage.setItem("token",data.token)
                goto('/dashboard');
            } else {
                // Handle login failure
                alert(data.message);
            }
        } catch (error) {
            console.error('Error:', error);
        }
	};

	const handleChange = (event) => {
		userType = event.target.value;
		resetFormData();
	};

	const resetFormData = () => {
		formData = {}; // Reset formData when user type changes
	};
</script>

<main>
	<label for="userType">Select User Type:</label>
	<select id="userType" bind:value={userType} on:change={handleChange}>
		{#each userTypes as type}
			<option value={type}>{type}</option>
		{/each}
	</select>

	{#if userType === 'Admin'}
		<form on:submit={handleSubmit}>
			<label for="admin_email">Admin Email:</label>
			<input type="email" id="admin_email" bind:value={formData.admin_email} required />

			<label for="admin_password">Admin Password:</label>
			<input type="password" id="admin_password" bind:value={formData.admin_password} required />

			<button type="submit">Login</button>
		</form>
	{:else if userType === 'User'}
		<form on:submit={handleSubmit}>
			<label for="user_email">User Email:</label>
			<input type="email" id="user_email" bind:value={formData.user_email} required />

			<label for="user_password">User Password:</label>
			<input type="password" id="user_password" bind:value={formData.user_password} required />

			<button type="submit">Login</button>
		</form>
	{:else if userType === 'Dealership'}
		<form on:submit={handleSubmit}>
			<label for="dealership_email">Dealership Email:</label>
			<input type="email" id="dealership_email" bind:value={formData.dealership_email} required />

			<label for="dealership_password">Dealership Password:</label>
			<input
				type="password"
				id="dealership_password"
				bind:value={formData.dealership_password}
				required
			/>

			<button type="submit">Login</button>
		</form>
	{/if}
    <div class="move">
        <a href="/">Home</a>
        <a href="register">Register</a>
    </div>
</main>

<style>
    main{
        max-width: 400px;
        margin: 0 auto;
        padding: 20px;
    }

    label {
        display: block;
        margin-bottom: 10px;
    }

    select {
        width: 100%;
        padding: 10px;
        margin-bottom: 20px;
    }

    .form {
        margin-bottom: 20px;
    }

    .move{
        width: 100%;
        height: 80px;
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    .move a{
        text-decoration: none;
        font-size: 18px;
        background-color: #007bff;
        padding: 5px 15px;
        border: none;
        border-radius: 5px;
        color: white;
    }

    input[type="text"],
    input[type="email"],
    input[type="password"],
    input[type="number"],
    input[type="tel"] {
        width: 100%;
        padding: 10px;
        margin-bottom: 20px;
        border: 1px solid #ccc;
        border-radius: 5px;
    }

    button[type="submit"] {
        background-color: #007bff;
        color: #fff;
        border: none;
        padding: 10px 20px;
        border-radius: 5px;
        cursor: pointer;
    }

    .links {
        display: flex;
        justify-content: space-between;
    }

    .links a {
        color: #007bff;
        text-decoration: none;
    }

    .links a:hover {
        text-decoration: underline;
    }
</style>
