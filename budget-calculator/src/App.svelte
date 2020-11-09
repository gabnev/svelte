<script>
	// bringing setContext from svelte
	import {setContext} from 'svelte'

	// Example: setting object to hold the information
	const state = {
		remove: removeExpense
	}

	// Components
	import Navbar from './Navbar.svelte';
	import ExpensesList from './ExpensesList.svelte';
	import Totals from './Totals.svelte'
	import ExpenseForm from './ExpenseForm.svelte'
	
	// Data
	import expensesData from './expenses';

	// Variables & functions
	let expenses = [...expensesData];

	// reactive variable
	$: total = expenses.reduce((accumulator, current)=>{
		return (accumulator += current.amount)
	}, 0)

	function removeExpense(id) {
		expenses = expenses.filter(expense => expense.id != id)
		console.log(expenses)
	}

	function clearExpenses() {
		expenses=[];
	}

	// Creating the setContext content
	setContext('remove', removeExpense)

</script>

<Navbar />

<main class="content">
	<ExpenseForm />
	<Totals title="Total Expenses" total={total}/>
	<ExpensesList {expenses} />
	<button type="button" class="btn btn-primary btn-block" on:click={clearExpenses}>Clear Expenses</button>
</main>
