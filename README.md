# Getting Started with Create React App

This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).


### Expense Tracker Github Build Process.

### install dependencies - npm i (bootstrap / uuid / react-icons)

-- app.js - import 'bootstrap/dist/css/bootstrap.min.css'

### Step 1.
create the three components which would contain the Budget, Remaining and Total Expenses Value

### Step 2.
Create the Expense List Componen. 
	- create a dummy array list of items eg.
	
	const expenses = [
        {id: 12, name: Grocery, cost: 50},
        {id: 13, name: Beverages, cost: 700},
        {id: 14, name: Airtime, cost: 100},
        ]
	
	now render the jsx (return)
	
	<ul>
	{expenses.map((individualExpense) => {
	return (
	<ExpenseItem id = {individualExpense.id} name = {individualExpense.name} cost = {individualExpense.cost}
	))}
	 </ul>

###Step 3.
Create the ExpenseItem Component from Step 2. This would be how the data gotten from ExpenseList would be displayed in the UX
	- it would accept props from ExpenseList
	
Step 4.
Create the AddExpenseForm component

	  <div className="col-sm">
             <label htmlFor="name">Name</label>
             <input type="text" required className='form-control' id='name' />
          </div>

###Step 5.
Create a Context component that'll hold the global states - 
	- create the initial states of the items on the list
	- import createContext from react
	- export the component and assign it to the createContext hook 
	- create AppProvider function which would accept props. It literally holds the state and passes it to the components. to hold the state, the useReducer hook is used.
	- import useReducer hook. The useReducer hook accepts gives us the current state and a dispatch func which would be used to update the state(dispatch actions)...similar to the useState hook.
	- whenever the useReducer is called, it accepts a reducer (AppReducer) and an initial state whenever the app first renders / runs.
	- create the AppReducer...this is in charge of updatimg the state based on certain actions that it rceives. This accepts the cirrent global state which is passed in by react and an action courtesy of the dispatch. 
	 The reducer uses a switch statement to determine how to update the state which depends on what is stated in the (action.type).
	- Lastly you have to return some of the state objects from the AppProvider so that the connected components can get access to them. These include the budget, expenses and dispatch. Also ensure to include {props.children} to take care of nested components.

### Step 6.
import the AppProvider in App.js. Create an <AppProvider> tag, close it and wrap all pre-existing jsx into it.

### Step 7.
Now that the context is setup, we begin to make changes to each component
	- import useContext into the component eg Budget.jsx
	- import AppContext into the component
	- using useContext, import the data from AppContext eg. const {budget} = useContext(AppContext)

	- apply same method to remaining.jsx
	- now the budget and expense would be needed eg const {budget, expense} = useContext(AppContext)
	- create a totalExpense variable
	
	const totalExpense = expense.reduce((total, item) => {
	return (
	(total = total + item.cost)
	}}, 0) 

###Step 8.
Adding a new data to the list. we make use of the useState hook.
	Setting the init state values as empty strings. Add a value propertyto the input which would be same as the initial state name
	Include an onchange function to take care of modifying the state. eg onChange={(event) => setCost(event.target.value)}
	Import AppContext. Using useContext(AppContext) inport the dispatch. This is because you want to make changes to the existing state.
	Create a new expense list, setting the id to uuidv4, name to name and cost to parseInt(cost) --- x1
	Create the dispatch function which would take in objects type and payload eg.  type: 'ADD_EXPENSE', payload: expense. --- x2
	Note: x1 and x2 are contained in the onSubmit function

	Add the dispatch type as a case to theswitch in AppContext

###Step 9.
Deleting an ExpenseItem
	- import AppContext and useContext in the ExpenseItem componen. Bring in the {dispatch} due to changes to be made to existing state.
	- create the delete function passing in the dispatch which would accept action type and payload as objects eg.
	
	const handleDelete = () => {
	dispatch({
	type: DELETE_EXPENSE,
	payload: props.id
	})
	}

	create the delete case in the AppContext eg
	
	case DELETE_EXPENSE: 
	return (
	...state,
	expenses: state.filter((expense) => {
	return (
	expense.id !== action.payload
	)})
	)


	###...more functionalities to be added with time
	



























