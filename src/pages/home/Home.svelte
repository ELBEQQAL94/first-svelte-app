<script>
import {onMount} from 'svelte';
import PostForm from './PostForm.svelte';

let posts = [];
let editingPost = {
    title: '',
    body: '',
    id: null
}
onMount(async() => {
    const fetchData =  await fetch(`https://ndb99xkpdk.execute-api.eu-west-2.amazonaws.com/dev/posts`);
    posts = await fetchData.json();
});

// edit post
function editPost(post) {
    editingPost = post;
};

// delete post
function deletePost(id){
    if(confirm("Are you sure?")){
        fetch(`https://ndb99xkpdk.execute-api.eu-west-2.amazonaws.com/dev/post/${id}`, {
        method: 'DELETE'
    })
    .then(res => {
        return res.json()
    })
    .then(() => posts = posts.filter(p => p.id !== id))
    }
};

// add post 
function addPost({detail: post}){
    if(posts.find(p => p.id === post.id)){
        let index = posts.findIndex(p => p.id === post.id);
        let postUpdated = posts;
        postUpdated.splice(index,1,post);
        posts = postUpdated;
    } else posts = [post,...posts];

    editingPost = {
        title: '',
        body: '',
        id: null
    };
}

</script>

<h1>Welcome Home !</h1>

<div class="row">
    <div class="col s6">
       <PostForm on:Postcreated={addPost} {editingPost} />
    </div>
</div>

<div class="row">
  {#if posts.length === 0}
    <p>Loading posts...</p>
  {:else}
     {#each posts as post}
        <div class="col s6">
          <div class="card">
             <div class="card-content">
              <p class="card-title">{post.title}</p>
              <p>CreatedAt: {post.createdAt}</p>
              <p>{post.body}</p>
             </div>
             <div class="card-action">
             <a href="#" on:click={() => editPost(post)}>Edit Post</a>
             <a href="#" class="delte-btn" on:click={() => deletePost(post.id)}>Delete Post</a>
             </div>
          </div>
        </div>
     {/each}
    {/if}
</div>