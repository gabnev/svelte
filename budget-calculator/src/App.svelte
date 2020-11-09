<script>
	// bringing setContext from svelte
	import {setContext} from 'svelte'

	// Components
	import Navbar from './Navbar.svelte';
	import ExpensesList from './ExpensesList.svelte';
	
	// Data
	import expensesData from './expenses';

	// Variables & functions
	let expenses = [...expensesData];

	function removeExpense(id) {
		expenses = expenses.filter(expense => expense.id != id)
		console.log(expenses)
	}

	// Creating the setContext content
	setContext('remove', removeExpense)
</script>

<Navbar />

<main class="content">
	<ExpensesList {expenses} />
</main>
