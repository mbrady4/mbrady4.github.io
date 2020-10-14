# Backend vs. Frontend

As always, such decisions involve a trade-off between different goals, some of which conflict with each other.

- Efficiency would suggest that you perform calculations in the front-end - both because that way the user's computer uses more power and your server uses less, and because the user sees faster feedback, which improves the user experience.
- Security demands that any state-changing operations **cannot** rely on data being checked or computed in the client computer, because the client computer may be under the control of a malicious attacker. Therefore, you **must** validate anything that comes from untrusted sources server-side.
- Programming efficiency and maintainability suggests that you shouldn't do the same computation twice because of the wasted effort.

From [https://softwareengineering.stackexchange.com/questions/252224/when-is-it-appropriate-to-do-calculations-in-front-end](https://softwareengineering.stackexchange.com/questions/252224/when-is-it-appropriate-to-do-calculations-in-front-end):

**There are strong reasons for doing calculations in the backend**

- Business logic doesn't belong in the presentation layer
- Business logic in JavaScript poses a threat
- **You assume there's a one-front-end -> one-back-end relationship which is not always true**, back-ends should be thought of as being capable or serving more than one front-end application, thus you cannot assume anything.
- If calculations use a big ammount of data, it wouldn't be efficient to hold it in the front-end
- If calculations uses the database, you will not be able to replicate it in the front end

R**ecommendation**

- Have the database enforce as much business rules as posible in the model, including foreign keys, primary keys, check constraints and triggers
- Have the business layer throw exceptions when business rules are not met (either because the database returned an error or because the business layer itself validated the data )
- If and only if response time is unacceptable, do validations or preprocessing using Ajax ( the work will not be really done in JavaScript, it will be done in the backend without having to reload the page)
- You can do simple validation in JavaScript like not allowing an empty value, or values that are too long, or out of range (for the latter you may want to generate the ranges in the back-end )