<script>
  import MainHeader from './lib/MainHeader.svelte';
  import PostsList from './lib/PostsList.svelte';

  let modalIsVisible = $state(false);
</script>

<MainHeader bind:modalIsVisible />
<main>
  <PostsList bind:modalIsVisible />
</main>

<style>
</style>
