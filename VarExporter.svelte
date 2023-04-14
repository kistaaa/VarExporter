<script>

let types =["strong typed", "let", "const", "var", "nothing"]
let defType = types[0]
let semi = true
let demo = [["message", "hello world"], ["width", "10"], ["height", "4.5"], ["is_on", "true"], ["is_off", "false"], ["numbers", "[1, 2, 3]"], ["words", '["hello", "world"]'], ["position", '{"x":1, "y":2}']]

function findType(str){
	const isInteger = Number.isInteger(Number(str));
	const isFloat = !isNaN(parseFloat(str));
	let isArray;
	let isObject;
	let isBoolean;
	
	try {
		isArray = Array.isArray(JSON.parse(str));
	} catch (e) {
		isArray = false;
	}
	
	try {
		isObject = (typeof JSON.parse(str) === "object");
	} catch (e) {
		isObject = false;
	}
	
	try {
		isBoolean = (JSON.parse(str) === true || JSON.parse(str) === false);
	} catch (e) {
		isBoolean = false;
	}
	
	if(isInteger) return 'int'
	if(isFloat) return 'float'
	if(isArray) return 'array'
	if(isObject) return 'object'
	if(isBoolean) return 'boolean'
	return 'string'
}
	
</script>

<header>
<h1>
	VarExporter
	</h1>
	<p>This simple app is mainly for learning/teaching purposes. Just edit the variable-value pairs, and it will diferentiate strings, numbers, booleans, arrays and objects, and generate code based on the options provided.</p>
	<p><i>Let, const and var:</i> are for Javascript and other languages. Remember that 'const' is immutable.</p>
	<p><i>Nothing:</i> is for languages like Python and Ruby.</p>
	<p><i>Strong typed:</i> is for languages like java, c#, c, c++. This script will give some guidance about the types, but the actual names and structures used in each language are different, for example some languages use 'bool' instead of 'boolean'. Also, arrays and objects are created diferently.</p>
	<p><i>Semicolons:</i> Automatic semicolons for languages that won't work without them.</p>
	Code on Github: <a href="https://github.com/kistaaa/VarExporter" target="_blank">VarExporter</a>
</header>

<section>
  
<aside>
	<div class="var">
		<div>Variable:</div> <div>Value:</div>
	</div>

	{#each demo as v}
	<div class="var">
		<input bind:value={v[0]}> <input bind:value={v[1]}>
	</div>
	{/each}
	*arrays and objects have to be valid json for automatic detection in 'strong typed'
</aside>

<main>
	<h3>
		Options:
	</h3>
	{#each types as type}
		<label>
		<input type=radio bind:group={defType} name="types" value={type}>
		{type}
		</label>
	{/each}
	
	<label>
	<input type=checkbox bind:checked={semi}>
	Use Semicolons?
	</label>
	<h3>
		Code:
	</h3>
	<code>
		{#each demo as line}
		{#if defType != 'strong typed'}
		
			{#if line[1]}
			{(defType != 'nothing' ? defType : '')} {line[0]} = {findType(line[1]) == 'string' ? '"' : ''}{line[1]}{findType(line[1]) == 'string' ? '"' : ''}{(semi  ? ';' : '')} {'\n'}
			{:else}
			{(defType != 'nothing' ? defType : '')} {line[0]}{(semi  ? ';' : '')} {'\n'}
			{/if}
		
		{:else}
		
			{#if line[1]}
			{findType(line[1])} {line[0]} = {findType(line[1]) == 'string' ? '"' : ''}{line[1]}{findType(line[1]) == 'string' ? '"' : ''}{(semi  ? ';' : '')} {'\n'}
			{:else}
			{findType(line[1])} {line[0]}{(semi  ? ';' : '')} {'\n'}
			{/if}
		
		{/if}
		{/each}
	</code>
</main>
  
</section>

<style>
	section {display:flex; margin:0; padding:0}
	aside, main {flex:1; padding:20px}
	aside {border-right:1px solid #ccc}
	code {border:1px solid #ccc; width:100%; padding:6px; display:block; white-space: pre-line}
	.var {display:flex}
	.var input, .var div{flex:1}
	.var div{font-weight:bold;}
</style>
