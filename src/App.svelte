<script>
  import ContactCard from "./ContactCard.svelte";

  let name = "";
  let title = "";
  let image = "";
  let description = "";
  let formState = '';
  let pass = '';
  let createdContacts = [];
  let enteredPasswords = [];


  function addContact() {
    if( name.trim().length == 0 || title.trim().length == 0 || image.trim().length == 0 || description.trim().length == 0) {
      formstate = 'invalid';
      return;
    }
    createdContacts = [...createdContacts, {name, jobTitle: title, imageUrl: image, description, id: createdContacts.length +1}];
    formState = 'done';
  }

  function deleteFirst() {
    createdContacts = createdContacts.slice(1);
  }
  function deleteLast() {
    createdContacts = createdContacts.slice(0, -1);
  }

  function addPassword() {
    if (pass.length >= 5 && pass.length <= 10) {
      enteredPasswords = [...enteredPasswords, {password: pass, id: enteredPasswords.length + 1}];
    }
  }

  function deletePassword(item) {
    enteredPasswords = enteredPasswords.filter(value => value.id !== item.id);
  }
</script>

<style>
  #form {
    width: 30rem;
    max-width: 100%;
  }

  .text-italic {
    font-style: italic;
  }
</style>

<div id="form">
  <div class="form-control">
    <label for="userName">User Name</label>
    <input type="text" bind:value={name} id="userName" />
  </div>
  <div class="form-control">
    <label for="jobTitle">Job Title</label>
    <input type="text" bind:value={title} id="jobTitle" />
  </div>
  <div class="form-control">
    <label for="image">Image URL</label>
    <input type="text" bind:value={image} id="image" />
  </div>
  <div class="form-control">
    <label for="desc">Description</label>
    <textarea rows="3" bind:value={description} id="desc" />
  </div>
  <div class="form-control">
    <label for="pass">Password</label>
    <input type="password" bind:value={pass} id="pass" />
  </div>
  {#if pass.length < 5}
    <p>Password is too short</p>
  {:else if pass.length > 10}
    <p>Password is too long</p>
  {:else if pass.length >= 5 && pass.length <= 10}
    <p>this is your password: {pass}</p>
  {/if}
</div>

<button on:click={addContact}>Add Contact Card</button>
<button on:click={deleteFirst}>Delete first</button>
<button on:click={deleteLast}>Delete last</button>
<button on:click={addPassword}>Add Password</button>

{#if formState === 'invalid'}
  <p>Invalid input</p>
{:else if formState === 'done'}
{:else}
  <p>Please input your information</p>
{/if}


{#each createdContacts as contact, i (contact.id)}
  <h2># {i + 1}</h2>
<!--  <ContactCard userName={contact.name} jobTitle={contact.jobTitle} description={contact.description}-->
<!--               userImage={contact.imageUrl} />-->
  <p>{contact.name}</p>
  <p>{contact.jobTitle}</p>
  <p>{contact.description}</p>
  <p>{contact.imageUrl}</p>
{:else}
  <p>Please start adding some contacts, we found none</p>
{/each}

{#if enteredPasswords.length > 0}
  <h2>Your entered passwords are:</h2>
{/if}

<ul>
  {#each enteredPasswords as item (item.id)}
    <li on:click={() => deletePassword(item)}>{item.password}</li>
  {:else}
    <p class="text-italic">You haven't saved any passwords yet</p>
  {/each}
</ul>


