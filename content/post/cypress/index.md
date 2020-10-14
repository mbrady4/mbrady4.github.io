+++
title = "Cypress"

date = 2020-10-14T00:00:00
lastmod = 2020-10-14T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
Software testing uses the general framework of: 

1. Arrange - Setup initial app state. Set up a webpage, form input, etc.
2. Action - take an action. Simulate a user action, like a button click or form input
3. Assert- make an assertion, verify that the simulated user action resulted in the expected output.

There are four main types of testing: 

1. Static Tests: Catch types of errors as you type (IDE)
2. Unit: Verifies individual, isolated parts of code (e.g., functions) work as expected. 
3. Integration: Works to test several units at one time — verifying they work together as expected. For example, if you have a function that relies on the output of another function, you might write an integration test to confirm that they’re working together as expected.
4. End to End: End to end testing basically asks “can a user accomplish an action?”. End to end tests focus on UI and mimic how a user might interact with an app, simulating real events like button clicks, scrolls, form submits, and the like.

## Cypress

Install Cypress in terminal in project folder with:

`npm install --save-dev cypress`

Open Cypress with:

`npx cypress open`

Syntactically, testing in Cypress looks something like this: Every test will start with a describe higher order function, and will accept a test as a callback function. Within the callback function there will be some it statement, as well as actions and assertions.

- `cy.visit()` will simulate a user visiting a page.
- `expect()` will verify if some expectation is met.

```jsx
describe('Name Test', function () {

    it('Explain what it does', function() {

        // actions and assertions go here
    })
})
```

## Cypress Example

```jsx
describe('Test our form inputs', function () {

    beforeEach( function () {
        cy.visit('http://localhost:3000/');
    });

    it("adds text to inputs", function () {
       cy.get('[data-cy="name"]').type("Mike").should("have.value", "Mike");
       cy.get('[data-cy="email"]')
          .type("mikebrady44@gmail.com")
          .should("have.value", "mikebrady44@gmail.com");
       cy.get('textarea')
         .type('I want to help')
         .should("have.value", "I want to help")
       cy.get('#positions')
         .select("Yard Work")
         .should("have.value", "Yard Work");
       cy.get('[type="checkbox"]').check().should("be.checked");
       cy.contains('Submit').click();
    //    cy.get('form').submit();
    });

    it("second it test", function () {

    });
});
```

# Cheatsheet

`npx cypress open` → open cypress GUI

`npx cypress run` → runs all cypress tests

`npx cypress run --spec "cypress/integration/form_test.js"` → runs the test file in the path after —spec

`describe` → use to start your test file and to group `it` tests

`it` → describes a test or series of assertions for your end-to-end test

`cy.get` → the selector from cy to get DOM element to interact with

`.type` → the function to enter code into selected, generally changed to a selected DOM element from get

`.should` → chained to some interaction (like type or check) and applies an assertion to test value

`.check()` → chained to checkbox DOM element

`.click` → chained to interactive element

`cy.screenshot('my-image')` → takes screenshot within test, useful for debugging

[Cypress Notes](https://www.notion.so/Cypress-Notes-a2aacf5a8a9048c9a8d7e6e8804e0a9e)

[Cypress.io](https://www.notion.so/Cypress-io-0885ff58b4594d958f48e288d0b18082)