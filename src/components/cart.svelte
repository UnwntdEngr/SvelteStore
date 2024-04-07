<script>
      import { myCart} from '../store/myOrder';
      import { thisArray } from '../store/itemsOut';
      import { forOut } from '../store/checkout';
      import {totalcost} from '../store/checkout';
      let newArray = [];
      let itemsTotal = $myCart.length;
      let total = $myCart.reduce((acc, item) => acc + item.price, 0).toFixed(2);
      $totalcost=[total];
      function updateArrayWithCounts() {
    const array = $myCart;
    let counts = {};
    array.forEach(product => {
      const key = `${product.name}-${product.price}`;
      counts[key] = (counts[key] || 0) + 1;
    });
    Object.entries(counts).forEach(([key, count]) => {
      const [name, price] = key.split('-');
      newArray.push({ name, price, count });
      $thisArray = [...newArray];
    });
  }
  updateArrayWithCounts();
  </script>

<main>
    <div id="cart-container">
      <button class="btn" id="clear-cart-btn" on:click={()=>{$myCart=[]}}>Clear Cart</button>
      <div id="products-container">
        <ul>
            {#each newArray as { name, price, count }}
              <li>{count > 1 ? `${count}x ${name} - ₱${price}` : `${name} - $${price}`}</li>
            {/each}
          </ul>
      </div>
      <hr class="border-gray-900 border-2">
      <p>Total number of items: <span id="total-items">{itemsTotal}</span></p>
      <p>Total: <span id="total">₱{total}</span></p>
      <hr class="border-gray-900 border-2">
      <button class="btn" on:click={()=>{$myCart.length===0?alert("Order First!!") :$forOut=true}}>Checkout</button>
    </div>
    
  </main>
