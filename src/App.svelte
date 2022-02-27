<script lang="ts">
	import * as duckdb from '@duckdb/duckdb-wasm';

	export let db: Promise<duckdb.AsyncDuckDB>;
	export let conn: Promise<duckdb.AsyncDuckDBConnection> = connection(); 
	export let res: Promise<Table<T>>;

	async function connection(){
		const conn = await (await db).connect();
		return conn;
	}
	
	async function query(query: string){
		const res = await (await conn).query(query);
		return res;	
	}
	
	res = query('SELECT count(*)::INTEGER as v FROM generate_series(0, 100) t(v)')
	
</script>

<main>

	{#await db}
		<h1>Instantiating...</h1>
	{:then db} 
		<h1>Hello DuckDB-Wasm {duckdb.PACKAGE_VERSION}</h1>
	{:catch error}
		<p style="color: red">{error.message}</p>
	{/await}
	{#await conn}
		<p>...waiting</p>
	{:then conn}
		<p>The connection is established.</p>
	{:catch error}
		<p style="color: red">{error.message}</p>
	{/await}
	{#await res}
		<h1>...executing</h1>
	{:then res} 
		<h1>{res.toArray()}</h1>
	{:catch error}
		<p style="color: red">{error.message}</p>
	{/await}

</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>