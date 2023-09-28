# expense-tracker
[Codecademy's](https://www.codecademy.com/learn) JavaScript project using [React-Redux](https://react-redux.js.org/) and [Redux Toolkit](https://redux-toolkit.js.org/).

## Expense Tracker
This project—a budgeting and expense tracking app—allows you to practice refactoring with Redux Toolkit. The app allows you to set budgets for various categories, such as food and transportation, and track transactions in those categories. It then sums your spending in each category to calculate the amount of money that remains to be spent.

To help you to understand how the data of the application works, consider an example of the Redux store’s state:
```js
{
  budgets: [ 
    { category: 'housing', amount: 400 },
    { category: 'food', amount: 100 },
    ...
  ],
  transactions: {
    housing: [ 
      { 
        category: 'housing', 
        description: 'rent', 
        amount: 400, 
        id: 123 
      }
    ],
    food: [ 
      { 
        category: 'food', 
        description: 'groceries on 1/12/2021', 
        amount: 50, 
        id: 456 
      },
      { 
        category: 'food', 
        description: 'dinner on 1/16/2021', 
        amount: 12, 
        id: 789 
      },
    ]
  }


}
```
You will work primarily in budgetsSlice.js and transactionsSlice.js where reducers and action creators are currently programmed by hand. Your task will be to refactor this project using a slice-based approach to produce the app’s actions and reducers.

Before you get started, spend some time using the app in its current implementation to ensure you understand how it’s supposed to work. Set a budget of $300 for food, create a $20 food transaction, and then check the food budget again to see how much you have left to spend. As you progress through the project, take note of the ways that Redux Toolkit simplifies your code.

## Tasks
### Create a Budgets Slice
1. Without Redux Toolkit, you have to write all your action creators and reducers by hand. Redux Toolkit’s createSlice() function generates action creators and reducers for you based on the inputs you give it.

Not only does createSlice() reduce the amount of code you have to write by automatically generating action creators and reducers, it also simplifies your reducers by allowing you to write mutating logic inside your reducers.

At the top of budgetsSlice.js:

- Import createSlice from @reduxjs/toolkit.

2. Next, you are going to define a slice by calling createSlice() with a configuration object containing the required name, initialState, and reducers properties.

- Define a variable, budgetsSlice, and initialize it with a call to createSlice(), passing in an empty configuration object. Do this right after the line defining initialState.
- Slices are conventionally named for the resource whose state they manage. This slice manages budgets and should be named accordingly. To give the slice a name, add a name property to the configuration object and set it equal to 'budgets'.
- Add an initialState property to the configuration object, and set it equal to the variable initialState that we’ve defined for you.
- Lastly, you’ll need to include a reducers property in the configurations object. For now, set it equal to an empty object.

3. In budgetsSlice.js, which we originally wrote without Redux Toolkit, you’ll see an editBudget() action creator. Currently, the action dispatched by that action creator will be processed in the 'budgets/editBudget' case of the budgetsReducer() we’ve provided. Open components/Budget.js where you can see this action being dispatched like so:
```js
dispatch(editBudget({category: budget.category, amount: amount}))
```
createSlice() automatically generates action creators and action types based on the case reducer functions we include in the reducers property. Once we define an editBudget case reducer, we will be able to delete our standalone action creators and reducers, and greatly simplify our code in the process.

- Add an editBudget property to the reducers object passed to createSlice().
- Set editBudget equal to a case reducer that receives two arguments—state and action . action.payload will have a category and amount property.
- editBudget should update the state by finding the budget object whose .category value matches action.payload.category and changing with the .amount value to action.payload.amount.

4. Now that you’ve implemented budgetsSlice, you’ll want to delete your old code and clean up your exports.

- Delete the stand-alone editBudget. At the bottom of the file budgetsSlice.js, export the editBudget action creator generated by createSlice() and stored in budgetsSlice.
- Delete the stand-alone budgetsReducer, and update the export default statement to export the reducer generated by createSlice() and stored in budgetsSlice.

Once you’ve completed this task, you should be able to edit budgets and see your changes reflected in the app.

### Create a Transactions Slice
5. Great work! Now that you’ve refactored the budgets slice with Redux Toolkit, you should do the same to the transactions slice, which is responsible for storing all of the user’s transactions organized by category. The slice handles adding new transactions and deleting existing transactions, and its state is an object structured like this:
```js
transactions = {
  housing: [ 
    { 
      category: 'housing', 
      description: 'rent', 
      amount: 400, 
      id: 123 
    }
  ],
  food: [ 
    { 
      category: 'food', 
      description: 'groceries on 1/12/2021', 
      amount: 50, 
      id: 456 
    },
    { 
      category: 'food', 
      description: 'dinner on 1/16/2021', 
      amount: 12, 
      id: 789 
    },
  ]
}
```
In transactionsSlice.js:

- Import createSlice from @reduxjs/toolkit.

6. Next, you are going to define a slice by calling createSlice() with a configuration object containing the required name, initialState, and reducers properties.

- Define a variable, transactionsSlice, and initialize it with a call to createSlice(), passing in an empty configuration object.
- Add a name property to the configuration object and set it equal to 'transactions'.
- Add an initialState property to the configuration object, and set it equal to the variable initialState that we’ve defined for you.
- Lastly, you’ll need to include a reducers property in the configurations object. For now, set it equal to an empty object.

7. Since we originally developed this project without Redux Toolkit, you’ll see two stand-alone action creators: addTransaction() and deleteTransaction(). Each of these action creators will receive an action.payload value that is a transaction object like so:
```js
transaction = {
  category: 'housing', 
  description: 'rent for January', 
  amount: 400, 
  id: 123
}
```
Open components/TransactionForm.js and components/Transaction.js where you can see how addTransaction() and deleteTransaction() are dispatched, respectively. Currently, the actions dispatched by these action creators will be processed by the transactionsReducer we’ve provided.

Your task is to replace these stand-alone action creators and the reducer with case reducers defined in the object passed to createSlice().

- Add an addTransaction property to the reducers object passed to createSlice().
- Set addTransaction equal to a case reducer that receives two arguments—state and action. It should add the new transaction object (action.payload) to the correct category’s transaction list in the transactions state object.
- Add a deleteTransaction property to the reducers object passed to createSlice().
- Set deleteTransaction equal to a case reducer that receives two arguments—state and action. It should delete the old transaction (action.payload) from the correct category’s transaction list in the transactions state object.

8. Now that you’ve implemented transactionsSlice, you’ll want to delete your old code and clean up your exports.

- Delete the stand-alone addTransaction and deleteTransaction, and export the addTransaction and deleteTransaction action creators generated by createSlice()and stored in transactionsSlice.
- Delete the stand-alone transactionsReducer, and update the export default statement to export the reducer generated by createSlice() and stored in transactionsSlice.

At this point, you should be able to add and delete transactions and see your changes reflected in the transactions list as well as in the “Funds Remaining” field for each budget.

# React + Vite

This template provides a minimal setup to get React working in Vite with HMR and some ESLint rules.

Currently, two official plugins are available:

- [@vitejs/plugin-react](https://github.com/vitejs/vite-plugin-react/blob/main/packages/plugin-react/README.md) uses [Babel](https://babeljs.io/) for Fast Refresh
- [@vitejs/plugin-react-swc](https://github.com/vitejs/vite-plugin-react-swc) uses [SWC](https://swc.rs/) for Fast Refresh
