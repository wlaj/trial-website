<script context="module">
    import { client } from '$lib/graphql-client'
    import { projectQuery } from '$lib/graphql-queries'
    import { marked } from 'marked'
  
    export const load = async ({ params }) => {
      const { slug } = params
      const variables = { slug }
      const { project } = await client.request(projectQuery, variables)
  
      return {
        props: {
          project,
        },
      }
    }
  </script>
  
  <script>
    export let project
  </script>
  
  <svelte:head>
    <title>My Portfolio | {project.name}</title>
  </svelte:head>
  
  <div>
    <img
      src={project.image[0].url}
      alt={project.title}
    />
  </div>
  
  <h1>{project.name}</h1>
  
  <div>
    <div>
      {#if project.tags}
        {#each project.tags as tag}
          <span
            >{tag}</span
          >
        {/each}
      {/if}
    </div>
  </div>
  
  <div
  >
    <a href={project.demo}>Demo</a>
    <a href={project.sourceCode}>Source Code</a>
  </div>
  
  <article>
    {@html marked(project.description)}
  </article>