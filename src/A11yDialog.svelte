<script>
export let id
export let closeButtonLabel = undefined
export let alertDialog = false

import A11yDialog from 'a11y-dialog'
import { onMount, onDestroy } from 'svelte'

export let dialog = undefined
let dialogElement

$: role = alertDialog ? 'alertdialog' : 'dialog'
$: titleId = $$slots.title ? `${id}-title` : undefined
$: descriptionId = $$slots.description ? `${id}-description` : undefined

onMount(() => {
	dialog = new A11yDialog(dialogElement)
	dialog.hide()
})
onDestroy(() => {
	dialog.destroy()
})
</script>

<div
	{id}
	class="dialog-root"
	bind:this={dialogElement}
	aria-hidden="true"
	aria-labelledby={titleId}
	aria-describedby={descriptionId}
	{role}
	on:show
	on:hide
	on:create
	on:destroy
>
	<div
		class="dialog-backdrop"
		data-a11y-dialog-hide={alertDialog ? undefined : ''}
	></div>
	<div
		role="document"
		class="dialog-content"
	>
		<button
			class="dialog-close-button"
			type="button"
			aria-label={closeButtonLabel}
			data-a11y-dialog-hide
		>
			<slot name="close-button">
				{ '\u00D7' }
			</slot>
		</button>
		{#if $$slots.title}
			<h1
				id={titleId}
				class="dialog-title"
			>
				<slot name="title" />
			</h1>
		{/if}
		{#if $$slots.description}
			<p
				id={descriptionId}
				class="dialog-description"
			>
				<slot name="description" />
			</p>
		{/if}
		<slot />
	</div>
</div>
