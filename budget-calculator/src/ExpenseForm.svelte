<script>
  import Title from "./Title.svelte";

  let name = "";
  let amount = 0;

  $: isEmpty = !name || !amount;
</script>

<section class="form">
  <Title title="Add Expense" />
  <form class="expense-form">
    <div class="form-control">
      <label for="name">Name</label>
      <input type="text" id="name" bind:value={name} />
    </div>
    <div class="form-control">
      <label for="amount">Amount</label>
      <input type="text" id="amount" bind:value={amount} />
    </div>
    {#if isEmpty}
      <p class="form-empty">Please fill out all form fields</p>
    {/if}
    <button type="submit" class="btn btn-block" disabled={isEmpty} class:disabled={isEmpty}>
      Add Expense
    </button>
    <button type="button" class="close-btn">
      <i class="fas fa-times" />
      Close
    </button>
  </form>
</section>
