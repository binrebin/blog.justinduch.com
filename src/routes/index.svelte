<script context="module">
	import { Card } from 'prisme-components-svelte';

	export function preload({ params, query }) {
		return this.fetch('article.json').then(r => r.json()).then(posts => {
			posts.sort(function(a, b){
				return new Date(b.date) - new Date(a.date);
			});

			return { posts };
		});
	}
</script>

<script>
	export let posts;
</script>

<style>
	a {
		text-decoration: none;
	}

	.container {
		display: flex;
		height: 100%;
	}

	.container .image {
		flex: 1 1 auto;
		border-radius: 5px 0 0 5px;
		display: block;
		max-width: 35%;
		height: auto;
	}

	.thumbnail {
		width: 100%;
		height: 100%;
	}

	.info {
		padding: 1em;
		position: relative;
		width: 100%;
		display: flex;
		flex-direction: column;
		justify-content: space-between;
	}

	.info .feet {
		display: flex;
		justify-content: space-between;
		width: 100%;
	}

	a {
		height: 8em;
	}

	@media (max-width: 600px) {
		a {
			height: 10em;
		}

		.container {
			flex-direction: column;
			height: auto;
		}

		.image {
			width: 100%;
			max-width: 100% !important;
		}

		.info {
			width: auto;
		}
	}

	@media (max-width: 345px) {
		.feet {
			flex-direction: column;
		}
	}
</style>

<svelte:head>
	<title>accueil</title>
</svelte:head>

<h2>beinvenue</h2>

{#each posts as post}
	<!-- we're using the non-standard `rel=prefetch` attribute to
			tell Sapper to load the data for the page as soon as
			the user hovers over the link or taps it, instead of
			waiting for the 'click' event -->
	<a rel='prefetch' href='article/{post.slug}'>
		<Card bs="md" m="0 0 1em 0" h="100%">
			<div class="container">
				<div class="image">
					<img src='https://cdn.halcyonnouveau.xyz/blog/thumbnails/{post.thumbnail}' alt='thumbnail' class='thumbnail'/>
				</div>
				<div class="info">
					<h4>{post.title}</h4>
					<div class="feet">
						<small>{post.readtime}</small>
						<small>from {post.category} on {post.date}</small>
					</div>
				</div>
			</div>
		</Card>
	</a>
{/each}
