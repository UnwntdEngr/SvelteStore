<script>
  import { writableArray } from "../store/index";
  import {myCart} from '../store/myOrder';
  import Cart from "./cart.svelte";
  import StorePage from "./storePage.svelte";
  let isCart = false;
</script>
<div class="bg-gray-900 p-5">
  <button id="cart-btn" type="button" class="btn" on:click={()=>{isCart?isCart=false:isCart=true}}>
    <span>{isCart?'Hide':'Show'}</span> Cart
  </button>
  {#if isCart}
  {#key $myCart}
  <Cart />
  {/key}
  {:else}
  <p> </p>
  {/if}
  <h1 class="text-center text-3xl my-10 text-white">Jei's Store</h1>
  
  <div class="grid grid-cols-2 md:grid-cols-3 flex-wrap items-center">
  {#each $writableArray as data}
  <div><StorePage {...data}/></div>
  {/each}
  </div>
</div>