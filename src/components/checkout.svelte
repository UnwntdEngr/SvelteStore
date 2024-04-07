<script>
    import { onMount } from 'svelte';
    import { thisArray } from '../store/itemsOut';
    import { readable } from 'svelte/store';
    import { totalcost } from '../store/checkout';
    let scriptUrl = 'https://script.google.com/macros/s/AKfycbzZFhtSv1Y8Cr1e9d8RGLB_zt3JXL0Tv_lBYCABKUDEtWJp59ET4ejPoW_RrVgvN39L/exec';
    let msg = "";
    const chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
  
    onMount(() => {
      const form = document.forms['submit-to-google-sheet'];
  
      form.addEventListener('submit', (event) => {
        event.preventDefault();
  
        fetch(scriptUrl, {
          method: 'POST',
          body: new FormData(form)
        })
          .then(response => {
            msg='Thank you for buying!! Please take note of your order ID: ' + $order_ID;
            alert(msg)
            location.reload()
          })
          .catch(error => console.error('Error!', error.message));
      });
    });
  
    function generateId() {
        let result = '';
        for (let i = 0; i < 20; i++) {
            result += chars[Math.floor(Math.random() * chars.length)];
        }
        return result;
    }
  
    let order_ID = readable(generateId());
  
    let formData = {};
    $: {
        formData = {
            ...formData,
            order_num: $order_ID,
            order: $thisArray.map(item => `${item.count}x ${item.name} - ₱${item.price}`),
            total_price:$totalcost
        };
    }
</script>

<div class="bg-gray-900 p-5 min-h-[100vh] text-center mx-auto">
    <div class="text-center my-5">
        <h1 class="text-white text-2xl md:text-4xl">Checkout</h1>
        <p class="text-white text-xl md:text-3xl">Please check your items:</p>
        <ul class="text-black bg-white text-md my-10">
            {#each $thisArray as { name, price, count }}
                <li>{count > 1 ? `${count}x ${name} - ₱${price}` : `${name} - ₱${price}`}</li>
            {/each}
            <hr class="border-2 border-gray-900">
            <p>TOTAL: {$totalcost}</p>
        </ul>
        
        <hr class="border-4 border-white">
        <h3 class="text-white text-2xl md:text-4xl">Please Input your information:</h3>
        <form name="submit-to-google-sheet">
            <input type="text" name="name" placeholder="Your Name" class="mt-2 md:my-2 text-sm md:text-xl text-black text-center flex-wrap w-[500px]" required><br>
            <input type="number" name="number" placeholder="Your Phone Number" class="mt-2 md:my-2 text-sm md:text-xl text-black text-center flex-wrap w-[500px]" required><br>
            <textarea name="message" rows="6" placeholder="Your Message" class="mt-2 md:my-2 text-sm md:text-xl text-black text-center flex-wrap w-[500px]"></textarea><br>
            {#each Object.keys(formData) as key}
                <input type="hidden" name={key} value={formData[key]} />
            {/each}
          
            <button type="submit" class=" btn text-center hover:bg-red-500 md:my-2 mx-auto">Submit</button>
        </form>
    </div>
</div>
