<script lang="ts">
  const { list }: { list?: string } = $props();
</script>

{#snippet level(items: Heading[])}
  {#if items.length}
    <ol class="list">
      {#each items as { id, index, text, children = [] }, i (id || i)}
        {@const indent = (index?.length || 1) - 1}
        <li style="--indent:{indent > 1 ? indent - 1 : 0}">
          {#if text && id}
            <a href="/#{id}">
              <span class="index">
                {index?.join(".")}
              </span>
              <span class="label">
                {text}
              </span>
            </a>
          {/if}
          {@render level(children)}
        </li>
      {/each}
    </ol>
  {/if}
{/snippet}

{#if list}
  {@const toc: Heading[] = JSON.parse(list || "[]")}
  <nav class="toc">
    <div class="inner">
      <h1 class="title">Table of Contents</h1>
      {#if toc.length}
        {@render level(toc)}
      {:else}
        <p><small style:color="red">Headings not found</small></p>
      {/if}
    </div>
  </nav>
{:else}
  <p><small style:color="red">List not found</small></p>
{/if}

<style>
  :global {
    .container-toc-and-content {
      display: grid;
      grid-template-columns: auto minmax(0, 800px);
      gap: 8px;
      max-width: 1200px;
      margin: 0 auto;

      nav.toc {
        position: sticky;
        top: 0;
        overflow: auto;
        max-height: 100vh;
      }
    }

    @media (max-width: 1200px) {
      .container-toc-and-content {
        grid-template-columns: 1fr;
        max-width: 800px;

        nav.toc {
          position: initial;
          max-height: initial;
        }
      }
    }

    .container-content {
      h1,
      h2,
      h3,
      h4,
      h5,
      h6 {
        display: flex;
        align-items: center;

        .content {
          flex: 1;
        }

        .act {
          gap: 0.5rem;
          display: inline-flex;
          flex-wrap: wrap;

          .section-link {
            font-size: 1rem;
            text-decoration: none;
            opacity: 0.75;
            transition: opacity 400ms;

            &::before {
              content: "§";
            }

            &:hover {
              opacity: 1;
            }

            *:hover > .act & {
              text-decoration: underline;
              text-underline-offset: 4px;
            }
          }
        }
      }
      h1,
      h2,
      h3,
      h4,
      h5,
      h6,
      p {
        max-width: 1200px;
      }
    }
  }

  nav.toc {
    .title {
      font-variant: all-small-caps;
      font-weight: bold;
      font-size: x-large;
    }
    .list {
      list-style: none;
      padding-left: 0;
      margin-left: 0;
      line-height: 1.5;

      li {
        li {
          font-size: 0.95em;
        }

        a {
          display: flex;
          text-decoration: none;
          color: inherit;

          .index {
            min-width: 5rem;
          }

          .label {
            margin-left: calc(var(--indent) * 1rem);
            flex: 1;
            border-bottom: 1px solid transparent;
          }

          &:hover .label {
            border-bottom-color: inherit;
          }
        }
      }

      .inner > & > li {
        margin-top: 1em;

        & > a {
          font-weight: bold;
        }
      }
    }
  }
</style>
