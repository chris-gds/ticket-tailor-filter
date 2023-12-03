<script>
	import { events } from '../eventData';
	import { categories } from '../eventData.js';
	import Event from './Event.svelte';
	import Icon from '$lib/filter.svg';

	let eventType = [];
	let selectedNames = ['all'];

	const filterSelection = (event) => {
		const clickedName = event.target.dataset.name;

		// Remove 'all' from the array once clicked
		selectedNames = selectedNames.filter((name) => name !== 'all');

		// Toggle array selection
		if (selectedNames.includes(clickedName)) {
			selectedNames = selectedNames.filter((name) => name !== clickedName);
		} else {
			selectedNames.push(clickedName);
		}

		// Restore 'all' if the array is empty
		if (selectedNames.length === 0) {
			selectedNames.push('all');
		}
	};
</script>

<svelte:head>
	<title>Order search</title>
</svelte:head>

<main>
	<aside class="layout__left">
		<fieldset>
			<legend
				><img src={Icon} class="icon" alt="" />
				Filter events
			</legend>

			<hr />

			{#each categories as category}
				<div class="filter__option">
					<label class="label" for={category}>{category}</label>
					<input
						bind:group={eventType}
						type="checkbox"
						value={category}
						data-name={category}
						id={category}
						on:click={filterSelection}
					/>
				</div>
			{/each}
		</fieldset>
	</aside>

	<div class="layout__right orders">
		<h1 class="layout__right-heading">Orders</h1>
		<p class="layout__right-para">Event(s) and description</p>

		<!-- Visual indicator of content -->
		<div class="outerwrapper" aria-hidden="true">
			<div class="inner__headings">
				<p class="inner__headings-copy">Event</p>
				<p class="inner__headings-copy">Client</p>
				<p class="inner__headings-copy">More details</p>
			</div>

			<hr />

			<Event>
				{#each events as { name, date, client, url, keyword, descr }}
					{#if selectedNames.includes('all') || selectedNames.includes(keyword)}
						<ul class="show column">
							<li class="content">
								<!-- Decorative image -->
								<img src={url} alt class="content__img" />
								<div class="content__event">
									<h2 class="content__heading">{name}</h2>
									<p class="content__date">{date}</p>
								</div>

								<h3 class="content__client">{client}</h3>
								<p class="content__paragraph">{descr}</p>
							</li>
						</ul>
					{/if}
				{/each}
			</Event>
		</div>
	</div>
</main>

<style>
	.outerwrapper {
		background: var(--lightGrey);
		border-radius: 2px;
		padding: 1rem;
	}

	.inner__headings {
		display: flex;
		flex-wrap: nowrap;
		align-content: center;
		justify-content: space-between;
		font-family: 'Coustard', serif;
		font-size: 0.8rem;
		padding: 0 0.5rem;
		margin: 0;
	}

	.inner__headings-copy {
		margin: 0;
		font-size: 0.8rem;
	}

	hr {
		border: 0;
		height: 0;
		border-top: 1px solid rgba(0, 0, 0, 0.1);
		border-bottom: 1px solid rgba(255, 255, 255, 0.3);
	}

	main {
		max-width: 80vw;
		display: flex;
		flex-direction: row;
		align-items: flex-start;
		justify-content: center;
		margin: 5rem auto 0 auto;
	}

	.column {
		width: 100%;
		justify-content: center;
		margin: 0;
		padding: 0;
	}

	.layout__left {
		width: 25%;
		padding: 1rem;
		background: var(--white);
		margin-right: 3%;
		border-radius: 0.5rem;
		border: 1px solid var(--mediumGrey);
	}

	.layout__right {
		width: 83%;
		border: 1px solid var(--mediumGrey);
	}

	.layout__right:after {
		content: '';
		width: 56px;
		height: 50px;
		background-image: url(mark.png);
		background-repeat: no-repeat;
		background-size: 56px 50px;
		background-position: 50%;
		top: 34px;
		right: 85px;
		position: absolute;
	}

	.layout__right-heading {
		margin: 0;
	}
	.layout__right-para {
		font-size: 0.9rem;
		margin: 0.25rem 0 1rem 0;
	}

	legend {
		font-family: 'Coustard', serif;
		font-size: 1.2rem;
		border-radius: 20%;
		display: flex;
		flex-direction: row;
		flex-wrap: nowrap;
		align-content: flex-start;
		justify-content: flex-start;
		align-items: center;
		line-height: 2rem;
	}
	.label {
		text-transform: capitalize;
		line-height: 1.8rem;
		margin-left: 0.7rem;
	}

	fieldset {
		border: 0;
		padding: 0;
	}

	.filter__option {
		display: flex;
		flex-wrap: nowrap;
		flex-direction: row-reverse;
		align-content: center;
		justify-content: flex-end;
		align-items: flex-start;
		margin-top: 1rem;
	}

	.icon {
		width: 2rem;
		margin-right: 0.5rem;
		margin-top: 0.4rem;
	}

	.content {
		background-color: var(--white);
		margin: 0.5rem 0;
		padding: 0.5rem;
		border-radius: 5px;
		width: 100%;
		box-shadow: 1px 1px 2.5px var(--mediumGrey);
		flex-direction: row;
		display: flex;
		flex-wrap: nowrap;
		align-content: flex-start;
		align-items: center;
	}
	.content__heading {
		font-size: 1rem;
		margin: 0;
	}
	.content__img {
		width: 15%;
		border-radius: 2px;
		aspect-ratio: auto;
		border: 1px solid var(--mediumGrey);
	}
	.content__event {
		width: 28%;
		margin-left: 1rem;
	}
	.content__date {
		font-size: 0.8em;
		margin: 0;
	}
	.content__client {
		width: 30%;
		font-size: 0.9em;
	}
	.content__paragraph {
		width: 27%;
		font-size: 0.8rem;
		/* number of lines to show limited to 2 */
		word-break: break-word;
		overflow: hidden;
		text-overflow: ellipsis;
		display: -webkit-box;
		-webkit-line-clamp: 2;
		-webkit-box-orient: vertical;
	}

	/* The "show" class is added to the filtered elements */
	.show {
		display: flex;
	}

	.orders {
		padding: 1rem;
		background: var(--white);
		border-radius: 0.5rem;
	}
</style>
