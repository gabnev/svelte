<script>
	// bringing setContext from svelte
	import { setContext, onMount } from "svelte";

	// Example: setting object to hold the information
	const state = {
		remove: removeExpense,
	};

	// Components
	import Navbar from "./Navbar.svelte";
	import ExpensesList from "./ExpensesList.svelte";
	import Totals from "./Totals.svelte";
	import ExpenseForm from "./ExpenseForm.svelte";

	// Data
	// import expensesData from "./expenses";

	// Variables & functions
	let expenses = [];

	// Editing
	let setName = "";
	let setAmount = null;
	let setId = null;

	// Toggle
	let isFormOpen = false;

	// reactive variable

	$: isEditing = setId ? true : false;

	$: total = expenses.reduce((accumulator, current) => {
		return (accumulator += current.amount);
	}, 0);

	// functions

	function showForm() {
		isFormOpen = true;
	}

	function closeForm() {
		isFormOpen = false;
		setName = "";
		setAmount = null;
		setId = null;
	}

	function removeExpense(id) {
		expenses = expenses.filter((expense) => expense.id != id);
		setLocalStorage();
	}

	function clearExpenses() {
		expenses = [];
		setLocalStorage();
	}

	function addExpense({ name, amount }) {
		let expense = { id: Math.random() * Date.now(), name, amount };
		expenses = [expense, ...expenses];
		setLocalStorage();
	}

	function setModifiedExpense(id) {
		
		let expense = expenses.find((item) => item.id === id);
		
		setName = expense.name;
		setAmount = expense.amount;
		setId = expense.id;		
		
		console.log(id);
		console.log(setId);
		showForm();
	}
	

	function editExpense({ name, amount }) {
		expenses = expenses.map((item) => {
			return item.id === setId
				? { ...item, name: name, amount: amount }
				: { ...item };
		});

		setId = null;
		setAmount = null;
		setName = "";
		setLocalStorage();
	}

	// Creating the setContext content
	setContext("remove", removeExpense);
	setContext("modify", setModifiedExpense);

	// local storage
	function setLocalStorage() {
		localStorage.setItem('expenses', JSON.stringify(expenses));
	}

	onMount(() => {
		expenses = localStorage.getItem('expenses') ? JSON.parse(localStorage.getItem('expenses')) : []
	})
</script>

<Navbar {showForm} />

<main class="content">
	{#if isFormOpen}
		<ExpenseForm
			{addExpense}
			name={setName}
			amount="{setAmount}"
			{isEditing}
			{editExpense}
			{closeForm} />
	{/if}
	<Totals title="Total Expenses" {total} />
	<ExpensesList {expenses} />
	<button
		type="button"
		class="btn btn-primary btn-block"
		on:click={clearExpenses}>Clear Expenses</button>
</main>
