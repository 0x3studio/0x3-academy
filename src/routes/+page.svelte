<script>
	import { createForm } from 'felte';
	import { validator } from '@felte/validator-yup';

	import schema from '../schemas/email';

	let serverSuccess = false;
	let serverError = false;
	let inputField;

	const { form, errors, isValid, reset } = createForm({
		onSuccess() {
			reset();
			serverError = false;
			serverSuccess = true;
		},
		onError() {
			serverError = true;
			serverSuccess = false;
		},
		extend: validator({ schema })
	});

	function refreshForm() {
		serverSuccess = false;
		serverError = false;
		reset();
	}
</script>

<svelte:head>
	<title>0x3 Academy</title>
	<meta property="og:title" content="0X3 Academy by 0X3 Studio" />
	<meta
		name="description"
		content="The best way to learn web3. From theory to pratical integration, for beginners and experts."
	/>
	<meta
		property="og:description"
		content="The best way to learn web3. From theory to pratical integration, for beginners and experts."
	/>
	<meta property="og:image" content="https://academy.0x3.studio/intro.png" />
</svelte:head>

<div class="container">
	<img src="/logo-0X3-academy.svg" alt="0x3 logo" class="w-36" />
	<h2>
		The best way to learn web3. <br />From theory to pratical integration, for
		<span class="gradient-text">beginners</span>
		and <span class="gradient-text">experts</span>.
	</h2>

	<div style="position:relative; height:10rem;">
		<!-- {#if !serverError && !serverSuccess} -->
		<p class="text-sm md:text-base mb-3">Get notified when available</p>
		<form use:form action="/api/save-email" method="post" class="flex mb-5 gap-x-2">
			<input
				bind:this={inputField}
				on:change={() => (serverSuccess = false)}
				type="text"
				name="email"
				placeholder="Enter your email"
				class="appearance-none block w-full text-gray-100 border border-gray-700 rounded py-3 px-4 leading-tight focus:outline-none focus:bg-white focus:border-gray-500"
			/>
			<button
				type="submit"
				disabled={!$isValid}
				class="shadow bg-purple-500 hover:bg-purple-400 focus:shadow-outline focus:outline-none text-white font-bold py-2 px-4 rounded disabled:pointer-events-none disabled:opacity-50"
				>Save</button
			>
		</form>
		<!-- {/if} -->
		<div>
			{#if $errors.email && inputField?.value.length > 0}
				<p class="text-sm md:text-base error">This is not a valid email address.</p>
			{:else}
				{#if serverError}
					<p class="text-sm md:text-base error">
						An error occurred while submitting your email address. <a
							href="/"
							style="text-decoration: underline;"
							on:click|preventDefault={refreshForm}>Please, try again</a
						>
					</p>
				{/if}
				{#if serverSuccess}
					<p class="text-sm md:text-base gradient-text">
						Thank you! We&apos;ll be in touch as soon as we&apos;re ready.
					</p>
				{/if}
			{/if}
		</div>
	</div>
</div>
<div style="position:absolute; bottom: var(--padding-layout); left: var(--padding-layout);">
	<a href="https://0x3.studio">
		<img
			src="/logo-0x3-studio.svg"
			width="134"
			alt="The beautiful and now famous logo of 0X3 studio"
		/></a
	>
</div>

<style>
	.error {
		color: #ff5edb;
	}
	.container {
		display: flex;
		flex-direction: column;
		gap: var(--padding-layout);
		justify-content: center;
		width: 100%;
		height: 100%;
		padding: var(--padding-layout);
		max-width: var(--max-width);
	}

	.gradient-text {
		background: linear-gradient(90.37deg, #8aff95 -3.24%, #31ceff 101.81%), #ffffff;
		-webkit-background-clip: text;
		-webkit-text-fill-color: transparent;
	}

	input,
	input:focus {
		background-color: #1a1a1a;
		color: #fff;
		border: 1px dotted #4399798b;
	}

	input::placeholder {
		color: rgba(255, 255, 255, 0.5);
	}

	button {
		color: #1a1a1a;
		background: linear-gradient(90.37deg, #8aff95 -3.24%, #31ceff 101.81%),
			linear-gradient(124.41deg, #8aff95 -3.2%, #2f04d8 84.13%), #3f945e;
	}
	h2 {
		font-size: var(--font-size-h2);
		font-weight: 500;
	}

	:root {
		--font-size-h2: 2em;
		--padding-layout: 5rem;
		--max-width: 680px;
	}

	@media (max-width: 768px) {
		:root {
			--font-size-h2: 1.5em;
			--padding-layout: 2.5rem;
			--max-width: 100%;
		}
	}
</style>
