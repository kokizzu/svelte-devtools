<script lang="ts">
	interface Props {
		axis: 'x' | 'y';
		children: import('svelte').Snippet;
	}

	let { axis, children }: Props = $props();

	let resizing = false;
	let size = $state(400);
</script>

<svelte:window
	onmouseup={() => (resizing = false)}
	onmousemove={({ pageX: x, pageY: y }) => {
		if (!resizing) return;
		size = axis === 'x' ? window.innerWidth - x : window.innerHeight - y;
	}}
/>

<aside class={axis} style="{axis === 'x' ? 'width' : 'height'}: {size}px">
	<!-- svelte-ignore a11y_no_static_element_interactions -->
	<div class="{axis} resize" onmousedown={() => (resizing = true)}></div>

	<div>{@render children()}</div>
</aside>

<style>
	aside {
		position: relative;
		display: grid;
		color: rgb(57, 63, 76);

		&.x {
			grid-template-columns: auto 1fr;
		}
		&.y {
			grid-template-rows: auto 1fr;
		}
	}

	.resize {
		top: 0;
		left: 0;
		width: 0.25rem;
		background: rgb(224, 224, 226);

		&:hover {
			background: rgb(177, 177, 179);
		}

		&.x {
			cursor: ew-resize;
			bottom: 0;
			width: 0.2rem;

			& + div {
				overflow-x: hidden;
			}
		}
		&.y {
			cursor: ns-resize;
			right: 0;
			height: 0.2rem;

			& + div {
				overflow-y: hidden;
			}
		}

		& + div {
			display: grid;
			gap: 0.625rem;
			align-content: flex-start;
			padding-top: 1rem;
		}
	}

	:global(.dark) {
		aside {
			background: rgb(36, 36, 36);
			color: rgb(177, 177, 179);
		}

		.resize {
			background: rgb(60, 60, 61);

			&:hover {
				background: rgb(107, 107, 108);
			}
		}
	}
</style>
