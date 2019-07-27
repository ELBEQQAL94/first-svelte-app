<script>
import {createEventDispatcher } from 'svelte';
export let editingPost;
const dispatch = createEventDispatcher();
$: title = editingPost.title;
$: body = editingPost.body;

  async function onSubmit(e){
      e.preventDefault();
      if(title.trim() === '' || body.trim() === '' ) return;
       
      const newPost = {
          title,
          body
      }

      let url, method;

      if(editingPost.id){
          url = `https://ndb99xkpdk.execute-api.eu-west-2.amazonaws.com/dev/post/${editingPost.id}`;
          method = 'PUT';
      } else {
          url = `https://ndb99xkpdk.execute-api.eu-west-2.amazonaws.com/dev/post`;
          method = 'POST'
      }

      const res = await fetch(url,{
          method,
          body: JSON.stringify(newPost)
      });
      const post = await res.json();

      dispatch('Postcreated', post);

      title = '';
      body = '';
  }
</script>

<form on:submit={onSubmit}>
   <div class="input-field">
     <label for="title">Title</label>
     <input type="text" bind:value={editingPost.title} />
   </div>

   <div class="input-field">
     <label for="body">Body</label>
     <input type="text" bind:value={editingPost.body} />
   </div>

   <button type="submit" class="btn waves-effect waves-light">
     {editingPost.id ? 'UPDATE' : 'SUBMIT'}
   </button>
</form>