## 🧪 Unit Testing

Unit testing is a way of testing the smallest blocks of code called **units.** Some examples of units are functions, methods, classes, and many more. This requires more development time to build these unit tests. So, why are they worth it?

### Benefits to implementing a unit testing solution

- allows for early error detection
- up to date documentation on the different units in the program
- improves quality of code by detecting any error-prone function 

### Workflow 

![image](https://github.com/user-attachments/assets/31037d5a-f6a8-4650-ba47-50e16c6c4203)

The following workflow describes how the unit testing would be implementing into our CI/CD pipeline. We have decided to implement the unit tests checks in Github Actions rather than Husky (pre-commit) due to two **core** reasons:

-  You want to run your tests in the same environment where you build your deliverables (deployed application). Pre-commits run the tests on the local environment which could be configured differently causing it to pass locally but fail on production server.

<br> 

-  Unit tests can be computationally expensive. Running them on a personal computer might cause performance issues.

### Resources

- This is a good article that shows how we can add unit testing for an API call: https://medium.com/@ben.dev.io/node-js-unit-testing-with-jest-b7042d7c2ad0
