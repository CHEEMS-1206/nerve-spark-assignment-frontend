<!-- Dashboard.svelte -->
<script>
    import { onMount } from 'svelte';

    let userType = '';
    let cars = [];

    onMount(async () => {
        // Fetch user type from localStorage
        userType = localStorage.getItem('user_type');
        console.log(userType);

        // Fetch cars data based on user type
        if (userType === 'Admin') {
            // No need to fetch cars for admin
        } else {
            try {
                const carsResponse = await fetch('http://localhost:5001/api/cars');
                cars = await carsResponse.json();
            } catch (error) {
                console.error('Error fetching cars:', error);
            }
        }
        console.log(cars);
    });
</script>

<main>
    {#if userType === 'Admin'}
        <h1>Welcome Admin</h1>
    {:else if userType === 'User' || userType === 'Dealership'}
        {#if cars.length > 0}
            <div>
                {#each cars as car}
                    <div class="car-card">
                        <h3>{car.name}</h3>
                        <p>Model: {car.model}</p>
                        <p>Type: {car.type}</p>
                        <p>Launch Date: {car.car_info.launch_date}</p>
                        <p>Price: ${car.car_info.price}</p>
                    </div>
                {/each}
            </div>
        {:else}
            <p>No cars available</p>
        {/if}
    {/if}
</main>

<style>
    .car-card {
        border: 1px solid #ccc;
        padding: 10px;
        margin-bottom: 10px;
        border-radius: 5px;
    }
</style>
