<script>
	export let showModal; 

	let dialog; 

	$: if (dialog && showModal) dialog.showModal();
</script>

<dialog
	bind:this={dialog}
	on:close={() => (showModal = false)}
	on:click|self={() => dialog.close()}
>
	<div on:click|stopPropagation>
		<slot name="header" />
		<hr />
		<slot />
		<hr />
		<button autofocus on:click={() => dialog.close()}>Apply</button>
	</div>
</dialog>

<style>
	dialog {
		max-width: 100%;
		border-radius: 0.2em;
		border: none;
		padding: 0;
		margin-top: 6rem;
	}
	dialog::backdrop {
	}
	dialog > div {
		padding: 1em;
	}
	dialog[open] {
		animation: zoom 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
	}
	@keyframes zoom {
		from {
			transform: scale(0.95);
		}
		to {
			transform: scale(1);
		}
	}
	dialog[open]::backdrop {
		animation: fade 0.2s ease-out;
	}
	@keyframes fade {
		from {
			opacity: 0;
		}
		to {
			opacity: 1;
		}
	}
	button {
		width: fit-content;
        height: 21px;
        font-family: 'Inter';
        font-style: normal;
        font-weight: 500;
        font-size: 19px;
        line-height: 115%;
        color: #2D3748;
        border: none; 
        background: transparent;
        border-radius: 6px;
        display: flex;
        flex-direction: row;
        justify-content: center;
        align-items: center;
        padding: 10px 8px;
        gap: 4px;
        width: 100px;
        height: 40px;
		background: #1A202C;
        border-radius: 6px;
        color: #FFFFFF;
		margin-left: 45rem;
	}
</style>
