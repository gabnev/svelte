<script>
  import {onMount, onDestroy, beforeUpdate, afterUpdate} from 'svelte'

  
  // beforeUpdate(() => {
  //   console.log('form is wating to be mounted')
  // })
  
  // onMount(() => {
  //   console.log('form has mounted')
  // })

  // afterUpdate(() => {
  //   console.log('form has been updated')
  // })
  
  // onDestroy(() => {
  //   console.log('form has been destroyed')
  // })
  
  import Title from "./Title.svelte";
  export let name = "";
  export let amount = 0;
  export let addExpense;
  export let isEditing;
  export let editExpense;
  export let closeForm;

  $: isEmpty = !name || !amount;

  function handleSumbit() {
    if (isEditing) {
      editExpense({ name, amount });
    } else {
      addExpense({ name, amount });
    }
    name = "";
    amount = null;
  }
</script>

<section class="form">
  <Title title="Add Expense" />
  <form class="expense-form" on:submit|preventDefault={handleSumbit}>
    <div class="form-control">
      <label for="name">Name</label>
      <input type="text" id="name" bind:value={name} />
    </div>

    <div class="form-control">
      <label for="amount">Amount</label>
      <input type="number" id="amount" bind:value={amount} />
    </div>

    {#if isEmpty}
      <p class="form-empty">Please fill out all form fields</p>
    {/if}

    <button
      type="submit"
      class="btn btn-block"
      disabled={isEmpty}
      class:disabled={isEmpty}>
      {#if isEditing}Edit Expense{:else}Add Expense{/if}
    </button>

    <button type="button" class="close-btn" on:click={closeForm}>
      <i class="fas fa-times" />
      Close
    </button>
  </form>
</section>
