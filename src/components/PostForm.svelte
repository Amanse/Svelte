<script>
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  let title = "";
  let body = "";
  let loading = false;

  const apiBaseUrl =
    "https://ndb99xkpdk.execute-api.eu-west-2.amazonaws.com/dev/post";

  async function onSubmit(event) {
    event.preventDefault();

    if (title.trim() === "" || body.trim() === "") {
      return;
    }

    loading = true;

    const newPost = {
      title,
      body
    };

    const res = await fetch(`${apiBaseUrl}`, {
      method: "POST",
      body: JSON.stringify(newPost)
    });
    title = "";
    body = "";
    const post = await res.json();
    dispatch("postCreated", post);
    loading = false;
  }
</script>

<style>
  form {
    margin: 50px;
  }

  .progress {
    margin: 100px 0;
  }
</style>

{#if !loading}
  <form on:submit={onSubmit}>
    <div class="input-field">
      <label for="title">Title</label>
      <input type="text" id="title" bind:value={title} />
    </div>
    <div class="input-field">
      <label for="body">Body</label>
      <input type="text" id="body" bind:value={body} />
    </div>
    <button type="submit" class="waves-effect waves-light btn">Add</button>

  </form>
{:else}
  <div class="progress">
    <div class="indeterminate" />
  </div>
{/if}
