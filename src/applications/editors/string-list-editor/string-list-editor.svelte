<script>
  import { ApplicationShell } from "@typhonjs-fvtt/runtime/svelte/component/core";
  import { getContext } from 'svelte';
  import { localize } from '@typhonjs-fvtt/runtime/svelte/helper';

  const { application } = getContext('#external');

  let form;

  export let elementRoot;
  export let stringList;

  function add() {
    stringList = [...stringList, ""];
    stringList = stringList;
  }

  function remove(index) {
    stringList.splice(index, 1)
    stringList = stringList;
  }

  async function updateSettings() {
    application.options.resolve(stringList);
    application.close();
  }

  export function requestSubmit() {
    form.requestSubmit();
  }

</script>

<svelte:options accessors={true}/>

<ApplicationShell bind:elementRoot>
	<form bind:this={form} on:submit|preventDefault={updateSettings} autocomplete=off>

		{#if application.options.content}
			<p>{localize(application.options.content)}</p>
		{/if}

		<table>
			<tr>
				<th>{localize(application.options.column)}</th>
				<th class="small"><a on:click={add} class="item-piles-clickable"><i class="fas fa-plus"></i></a></th>
			</tr>
			{#each stringList as path, index (index)}
				<tr>
					<td><input type="text" required bind:value="{path}"/></td>
					<td class="small">
						<button type="button" on:click={remove(index)}><i class="fas fa-times"></i></button>
					</td>
				</tr>
			{/each}
		</table>

		<footer>
			<button type="button" on:click|once={requestSubmit}>
				<i class="far fa-save"></i> {localize("Save")}
			</button>
			<button type="button" on:click|once={() => { application.close(); }}>
				<i class="far fa-times"></i> { localize("Cancel") }
			</button>
		</footer>

	</form>
</ApplicationShell>


<style lang="scss">

  table {
    vertical-align: middle;

    tr {
      border-spacing: 15px;
    }

    .small {
      width: 26px;
    }

    button {
      padding: 0.25rem 0.25rem;
      line-height: 1rem;
      flex: 0;
      text-align: center;
    }

    a {
      text-align: center;
    }
  }

</style>
