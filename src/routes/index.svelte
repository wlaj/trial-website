<script context="module">
    import ProjectCard from '$lib/components/project-card.svelte'
    import { client } from '$lib/graphql-client'
    import { authorsQuery, projectsQuery } from '$lib/graphql-queries'
  
    export const load = async () => {
      const [authorReq, projectsReq] = await Promise.all([
        client.request(authorsQuery),
        client.request(projectsQuery),
      ])
      const { authors } = authorReq
      const { projects } = projectsReq
  
      return {
        props: {
          projects,
          authors,
        },
      }
    }
  </script>
  
  <script>
    export let projects
    export let authors
  </script>

<div>
    {#each authors as { name, intro, picture: { url } }}
      <h1>{intro}</h1>
      <h1>{name}</h1>
      <img src={url} alt={name} />
    {/each}
  </div>
  
  <h1>Recent Projects by Me</h1>
  
  <div>
    {#each projects as { name, slug, description, image }}
      <ProjectCard {name} {description} url={image[0].url} {slug} />
    {/each}
  </div>