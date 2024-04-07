<script>
  import Checkout from "../components/checkout.svelte";
  import Database from "../components/database.svelte";
  import ShowPage from "../components/showPage.svelte";
  import { forOut } from "../store/checkout";
</script>
{#if $forOut}
<Checkout/>
{:else}
<ShowPage/>
<Database/>
{/if}

