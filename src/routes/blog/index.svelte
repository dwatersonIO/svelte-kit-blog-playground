<script context = "module">
    const allPosts = import.meta.glob("./*.{md, svx}")
    
    let body =[];
    for (let path in allPosts) {
        body.push(
            allPosts[path]().then(({metadata}) => {
                return { path, metadata };
        })
      );
    }
    
    export const load = async() => {
        const posts = await Promise.all(body);
        
        return {
            props: {
                posts,
            }
        }
    };
</script>

<script>
import About from "../about.svelte"

    export let posts
    const dateSortedPosts = posts.slice().sort((post1, post2) => {
        return new Date(post2.metadata.date) - new Date(post1.metadata.date)
    })
</script>


<div class="grid-container">

   <!-- {JSON.stringify(posts)} -->
    {#each dateSortedPosts as {path, metadata: {title, summary, tags, date} }}
        <article>
        <h1><a href={`/blog/${path.replace(".md","").replace(".svx", "")}`}>{title}</a></h1>
        <span class="date">{new Date(date).toDateString()}
            {#each tags as tag}
                <a class="tag high-col" href={`/tags/${tag}`}>#{tag} </a>
            {/each}
        </span>
        <h2 class="text-col">{summary}</h2>
        <div>
        <h6 ><a class="high-col" href={`/blog/${path.replace(".md","").replace(".svx", "")}`}>Read More...</a></h6>
        </div>
        </article>
    {/each}
</div> 
  
<style>

.tag {
    text-decoration: none;
    margin: .2em .2em;
    font-size: var(--p-font);
}
.tag:hover {
    color: var(--head-col)
}
.date {
    font-size: 0.7rem;
    color: grey
}

.tag-container {

}

</style>
    
    