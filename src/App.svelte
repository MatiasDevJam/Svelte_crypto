<script>
	
	  import { onMount } from 'svelte';
	
	  let titles = [
		'#',
		'Coin',
		'Price',
		'Price Change',
		'24h Volume'
	  ]
	
	  let coins = [];
	  let filteredCoins = []
	  let ref = null
	
	  const loadCoins = async() => {
	
		const res = await fetch('https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false')
		
		const data = await res.json()
		console.log(data);
		coins = data;
		filteredCoins = data;
	  }
	
	  loadCoins();
	
	  const searchCoin = ( value ) => {
		filteredCoins = coins.filter((coin) => 
		  coin.name.toLowerCase().includes(value.toLowerCase()) ||
		  coin.symbol.toLowerCase().includes(value.toLowerCase())
		)
	  };
	
	  onMount(() => {
		ref.focus()
	  })
	
	</script>
	
	<style>
	  :global(body) {
		background-color: #f2f2f2;
	  }
	  img {
		width: 1.5rem;
	  }
	</style>
	
	<main>
	  <div class="container">
		<div class="row">
		  
		  <h1 class="text-center mt-5 bg-primary">Mundo Crypto</h1>
	
		  
		  <div class="container">
			<div class="row col-4">
			  <input 
				type="text"
				class="form-control text-black rounded-pill border-5 my-4 "
				placeholder="Search your coin"
				on:keyup={({ target: { value } }) => searchCoin(value)}
				bind:this={ref}
			  >
			</div>
		  </div>
		 
	
		  <table class="table table-secondary mt-3">
			<thead>
			  <tr>
				{#each titles as title}
				  <th class="text-info">{ title }</th>
				{/each}
			  </tr>
			</thead>
			<tbody>
			  {#each filteredCoins as coin, i}
				<tr>
				  <td class="text-muted">{i + 1}</td>
				  <td>
					<img src={ coin.image } alt={ coin.name } class="img-fluid me-2"/>
					<span>{ coin.name }</span>
					<span class="text-muted text-uppercase ms-2">{ coin.symbol }</span>
				  </td>
				  <td>
					${ coin.current_price.toLocaleString() }
				  </td>
				  <td
					class={ coin.price_change_percentage_24h > 0
					  ? "text-success"
						:
					  "text-danger" }
				  >
					{ coin.price_change_percentage_24h }%
				  </td>
				  <td>
					${ coin.total_volume.toLocaleString() }  
				  </td>
				</tr>
			  {/each}
			</tbody>
		  </table>
		</div>
	  </div>
	
	</main>