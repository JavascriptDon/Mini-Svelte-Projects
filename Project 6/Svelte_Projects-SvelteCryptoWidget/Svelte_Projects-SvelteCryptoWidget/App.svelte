<script>
  import { onMount } from "svelte";

  let cryptoData = [];
  let crypto = [];
  let coins = [];
  let search = "";
  let ref = null;
  let titles = ["#", "Name", "Price", "Change (24hrs)(%)", "Market Cap"];

  const searchCrypto = value => {
    value = value.toLowerCase();
    cryptoData = crypto.filter(
      crypto =>
        crypto.name.toLowerCase().includes(value) ||
        crypto.symbol.toLowerCase().includes(value)
    );
  };

  onMount(async () => {
    const res = await fetch(
      `https://api.coingecko.com/api/v3/coins/markets?vs_currency=eur&order=market_cap_desc&per_page=100&page=1&sparkline=false`
    );
    const data = await res.json();
    console.log(data);
    crypto = data;
    cryptoData = data;
    ref.focus();
  });
</script>

<style>
  main {
    font-family: sans-serif;
    font-weight: bold;
    text-align: center;
    background-color: black;
    color: white;
  }
  input {
    width: 90%;
  }
</style>

<main>
<div class="container">
<div class="row">
<h1>Crypto Price Widget</h1>
</div>
</div>
<input bind:value={search} type="text" placeholder="Search Crypto...💰" on:keyup={({ target: { value } }) => searchCrypto(value)}   bind:this={ref}/>
<table class="table table-dark  table-hover my-4">
  <thead>
        <tr>
          {#each titles as title}
            <th class={title == "Coin" ? "px-3" : ""}>{title}</th>
          {/each}
        </tr>
      </thead> 
       <tbody> 
{#each cryptoData as crypto, i}  
<tr> 
<td>(⭐{crypto.market_cap_rank})</td>
<td>
 <img
                src={crypto.image}
                alt={crypto.name}
                style="width: 1.5rem"
                class="coin-image img-fluid mx-2"
              />
               <span class="ms-3">
                {crypto.name}
              </span>
              <span class="text-uppercase text-muted ms-2">
                ({crypto.symbol})
              </span>
              </td>
              <td>
              (${crypto.current_price.toLocaleString()})
            </td>
             <td
              class={crypto.price_change_24h > 0
                ? "bg-success text-white font-weight-bold"
                : "bg-danger text-white font-weight-bold"}
            >
              {crypto.price_change_percentage_24h}
            </td>
             <td>
              (${crypto.market_cap.toLocaleString()})
            </td>
            </tr>
               {/each}
          </tbody>
</table>   
</main>
