<script>
  import Modal from './Modal.svelte';
  import NewPost from './NewPost.svelte';
  import Post from './Post.svelte';
  import { Client, Databases, ID } from 'appwrite';
  import Spinner from './Spinner.svelte';

  let { modalIsVisible = $bindable() } = $props();
  let posts = $state([]);
  let isLoading = $state(false);

  const client = new Client();

  client
    .setEndpoint('https://cloud.appwrite.io/v1')
    .setProject('66eedd2f001dc1185030');
  const dbId = '66eee091000c6c0a41b0';
  const collId = '66eee09a003c7dca585b';
  const databases = new Databases(client);

  $effect(() => {
    async function getPosts() {
      try {
        isLoading = true;
        const results = await databases.listDocuments(dbId, collId);
        // console.log(posts.documents);

        posts = results.documents;
        isLoading = false;
      } catch (err) {
        console.error(err.message);
      }
    }
    getPosts();
  });

  async function handleCreateNewPost(data) {
    const id = ID.unique();
    try {
      isLoading = true;
      const result = await databases.createDocument(dbId, collId, id, data);
      const newPost = { ...data, $id: id };
      // console.log(result);
      posts.unshift(newPost);
      isLoading = false;
    } catch (err) {
      console.log(err.message);
    }
  }

  async function handleDeletePost(id) {
    try {
      console.log('indeletefunction');
      isLoading = true;
      const result = await databases.deleteDocument(dbId, collId, id);

      posts.splice(post => post.id === id);
      isLoading = false;
    } catch (err) {
      console.error(err.message);
    }
  }
</script>

{#if modalIsVisible}
  <Modal bind:modalIsVisible>
    <NewPost bind:modalIsVisible {handleCreateNewPost} />
  </Modal>
{/if}
{#if isLoading}
  <div style="text-align: center; color: white">
    <Spinner color="white" />
  </div>
{:else if posts.length}
  <ul class="posts">
    {#each posts as post (post.$id)}
      <Post author={post.author} onclick={() => handleDeletePost(post.$id)}
        >{post.body}</Post>
    {/each}
  </ul>
{:else}
  <div style="text-align: center; color: white">
    <h2>There are no posts yet.</h2>
    <p>Start adding some!</p>
  </div>
{/if}

<style lang="scss">
  @import '../styles/vars';

  .posts {
    list-style: none;
    max-width: 50rem;
    margin: 1rem auto;
    padding: 1rem 0;
    display: grid;
    gap: 1rem;
    grid-template-columns: repeat(3, 30%);
    justify-content: center;
  }
</style>
