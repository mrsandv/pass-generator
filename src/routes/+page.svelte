<script lang="ts">
	import { onMount } from 'svelte';
	import ReloadIcon from '../components/icons/reload.svelte';
	import CopyIcon from '../components/icons/copy.svelte';

	let length = 16;
	let useLower = true;
	let useUpper = true;
	let useNumbers = true;
	let useSymbols = false;
	let canCopy = false;
	let password = '';

	const LOWER = 'abcdefghijklmnopqrstuvwxyz';
	const UPPER = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ';
	const NUMBERS = '0123456789';
	const SYMBOLS = '!@#$%^&*()-_=+[]{};:,.<>?';

	function generatePassword() {
		let chars = '';
		if (useLower) chars += LOWER;
		if (useUpper) chars += UPPER;
		if (useNumbers) chars += NUMBERS;
		if (useSymbols) chars += SYMBOLS;

		if (!chars) {
			password = 'Selecciona al menos una opción';
			canCopy = false;
			return;
		}

		let result = '';
		const array = new Uint32Array(length);
		crypto.getRandomValues(array);

		for (let i = 0; i < length; i++) {
			result += chars[array[i] % chars.length];
		}

		password = result;
	}

	function handleCopy() {
		navigator.clipboard.writeText(password);
	}

	onMount(() => generatePassword());
</script>

<div>
	<div class="my-5 flex h-fit items-center justify-between">
		<p class="h-fit w-96 text-4xl font-semibold wrap-break-word">{password}</p>
		<div>
			<button class="mx-5 cursor-pointer hover:scale-105" onclick={handleCopy} disabled={canCopy}
				><CopyIcon /></button
			>
			<button class="cursor-pointer hover:scale-105" onclick={generatePassword}
				><ReloadIcon /></button
			>
		</div>
	</div>
	<div class="flex items-center justify-between">
		<div class="flex flex-col">
			Longitud:
			<label>
				{length}
				<input type="range" bind:value={length} min="10" max="40" onchange={generatePassword} />
			</label>
		</div>
		<div class="grid grid-cols-2 gap-2">
			<label
				><input type="checkbox" bind:checked={useLower} onchange={generatePassword} /> Minúsculas</label
			>
			<label
				><input type="checkbox" bind:checked={useUpper} onchange={generatePassword} /> Mayúsculas</label
			>
			<label
				><input type="checkbox" bind:checked={useNumbers} onchange={generatePassword} /> Números</label
			>
			<label
				><input type="checkbox" bind:checked={useSymbols} onchange={generatePassword} /> Símbolos</label
			>
		</div>
	</div>
</div>
