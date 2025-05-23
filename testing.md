## 🧪 Testing

Testing will help better the code quality of our codebase. We use the following tools to conduct our testings:

- Jest: allows to build testcases in Javascript/Typescript
- Supertest: allows developers to mock API responses

### Workflow 

![image](https://github.com/user-attachments/assets/31037d5a-f6a8-4650-ba47-50e16c6c4203)

The following workflow describes how the unit testing would be implementing into our CI/CD pipeline. We have decided to implement the unit tests checks in Github Actions rather than Husky (pre-commit) due to two **core** reasons:

-  You want to run your tests in the same environment where you build your deliverables (deployed application). Pre-commits run the tests on the local environment which could be configured differently causing it to pass locally but fail on production server.

<br> 

- Tests can be computationally expensive. Running them on a personal computer might cause performance issues.
