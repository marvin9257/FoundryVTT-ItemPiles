<script>
  import { ApplicationShell } from "@typhonjs-fvtt/runtime/svelte/component/core";
  import { localize } from '@typhonjs-fvtt/runtime/svelte/helper';
  import { getContext } from "svelte";

  const { application } = getContext('#external');

  export let elementRoot;

  let form;

  const users = game.users
    .filter(u => u.active && (u.character || u.isGM) && !(application.options.excludeSelf && u === game.user))
    .map(u => ({
      id: u.id,
      name: u.name,
      selected: true
    }));

  function requestSubmit() {
    form.requestSubmit();
  }

  function submit() {
    const finalUsers = users.filter(user => user.selected).map(user => user.id);
    application.options.resolve(finalUsers);
    application.close();
  }

</script>

<svelte:options accessors={true}/>

<ApplicationShell bind:elementRoot>

	<form class="item-piles-flexcol" bind:this={form} on:submit|once|preventDefault={submit} style="padding:0.5rem;"
				autocomplete="off">

		<p style="margin-top: 0;text-align: center;">
			<strong>{localize("ITEM-PILES.Dialogs.UserSelect.Content")}</strong>
		</p>

		<div style="display:grid; grid-template-columns: auto 25px;">

			{#each users as user (user.id)}

				<div class="item-piles-flexrow" style="align-items: center;">
					<label for={"item-piles-user-" + user.id} class="item-piles-clickable">{user.name}</label>
				</div>
				<input id={"item-piles-user-" + user.id} type="checkbox" bind:checked={user.selected}/>

			{/each}

		</div>

		<footer class="sheet-footer item-piles-flexrow" style="margin-top: 1rem;">
			<button type="button" on:click|once={requestSubmit}>
				<i class="fas fa-check"></i>
				{localize("Okay")}
			</button>
		</footer>

	</form>

</ApplicationShell>
