<script>
  import Modal from './Modal.svelte';

  let enteredBody = $state('');
  let enteredAuthor = $state('');

  let { modalIsVisible = $bindable(), handleCreateNewPost } = $props();

  function handleClearFields() {
    enteredAuthor = '';
    enteredBody = '';
  }

  function handleSubmit(e) {
    e.preventDefault();
    const formData = Object.fromEntries(new FormData(e.target));
    if (!formData.body || !formData.author) return;

    // console.log(formData);

    handleCreateNewPost(formData);
    handleClearFields();
    modalIsVisible = false;
  }
</script>

<form class="form" onsubmit={handleSubmit}>
  <p>
    <label for="body">Text</label>
    <textarea id="body" required rows={3} bind:value={enteredBody} name="body"
    ></textarea>
  </p>

  <p>
    <label for="name">Your name</label>
    <input
      type="text"
      id="name"
      required
      bind:value={enteredAuthor}
      name="author" />
  </p>
  <p class="actions">
    <button type="button" onclick={() => (modalIsVisible = false)}
      >Cancel</button>
    <button>Submit</button>
  </p>
</form>

<style lang="scss">
  @import '../styles/vars';

  .form {
    background-color: #6233b9;
    padding: 1rem;
    width: 20rem;
    /* margin: 2rem auto;
    border-radius: 6px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2); */
  }

  .form label {
    display: block;
    margin-bottom: 0.05rem;
    color: #eadbfb;
    font-weight: bold;
  }

  .form input,
  .form textarea {
    display: block;
    width: 100%;
    font: inherit;
    padding: 0.5rem;
    border-radius: 6px;
    border: none;
    background-color: #c4a9e4;
    color: #28262c;
  }

  .actions {
    display: flex;
    justify-content: flex-end;
    gap: 0.5rem;
  }

  .actions button {
    cursor: pointer;
    font: inherit;
    padding: 0.5rem 1.5rem;
    border: none;
    border-radius: 4px;
    background-color: #34036c;
    color: #e5d5f7;
  }

  .actions button:hover {
    background-color: #23014a;
  }

  .actions button[type='button'] {
    background-color: transparent;
  }

  .actions button[type='button']:hover {
    color: #d1bee6;
  }
</style>
