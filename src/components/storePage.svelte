<script>
  import { myCart } from '../store/myOrder';
export let name;
export let price;
export let category;
export let id;
</script>

<div class="dessert-card">
  <h2>{name}</h2>
  <p class="dessert-price">$ {price}</p>
  <p class="product-category">Category: {category}</p>
  <button 
  class="btn add-to-cart-btn" on:click={()=>$myCart=[...$myCart,{id,name,price}]}>Add to cart
</button>
</div>
