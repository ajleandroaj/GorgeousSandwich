# GorgeousSandwich

This repository serves as a method of training and learning for software development. The content present in this
repository represents the author's thoughts on best practices in software engineering. The project was created as a part
of Master's Degree program.

The project is divided into two parts. The first part focuses on a monolithic architecture, with a majority of the
functionalities implemented. The second part of the project involves migrating the monolithic architecture to a
microservice architecture and implementing the remaining functionalities.

# Project statements

* [**Statement 1**](docs/statements/statement_1.md): Monolith
* [**Statement 2**](docs/statements/statement_2.md): Microservice

# Analysis and requirements

## Domain model

All the components in the domain model diagram as created based
at [Statement 1](docs/statements/statement_1.md#21-more-possible-architectural-drivers). The
names and properties were named respecting the Ubiquitous Language.

![domain_model.png](docs/imgs/domain_model.png)

## Functional requirements

### Client

![use_cases_client.png](docs/imgs/use_cases_client.png)

1. **Sign-up**: A **Client** insert his name, email and authentication data to perform a sign-up.
2. **Login**: A **Client** insert his email and authentication data to perform a login.
3. **Order sandwiches**: A **Client** register sandwiches and their quantities for delivery on a specific day and shop.
   The total price is informed. A sandwich can never be sold below zero, despite the promotions applied.

### Admin

No details are provided at [Statement 1](docs/statements/statement_1.md#21-more-possible-architectural-drivers)
regarding who manage each component. In order to simplify, an Admin actor is used as a super role that can manage all
components.

![use_cases_admin.png](docs/imgs/use_cases_admin.png)

## Architecture drivers and quality attribute

1. The prototype to be developed must be accessible using a web browser and it should be available in the next four
   weeks.
2. Authentication is needed for the prototype. It must ensure that 99% of unauthorized login attempts are detected. For
   the prototype, the team must explore a solution that prevents brute-force authentication attempts and comes as close
   as possible to the specified value. Authorization will be introduced later.
3. The FoodSoftware wants to start considering metrics to improve the codebase. At this moment, no more than 5 metrics
   should be used, but the focus should be on what can bring benefits. At least one is mandatory to explore, an
   experimental
   metric ([A Promising New Metric To Track Maintainability](https://blog.hello2morrow.com/2018/12/a-promising-new-metric-to-track-maintainability))
   to indicate the maintainability level of the application. The Sonargraph-Explorer tool is able to calculate it. The
   team has carried out some experiments with the tool and aims to reach a value of at least 70% (see Figure 2.1). Other
   applications can be considered for other maintainability metrics.
4. Testing on various quality dimensions is mandatory for the prototype, including the business rules captured in domain
   layers and what can ensure the correct functioning of the application and its API. More than the number of tests, it
   is important to explain and document what the tests allow to verify, regardless of the type of tests, together with
   the analysis of the prototype with inclusion in the repository of specific reports of the applications used.
   Tests must be used in the demonstration.
5. Only open-source tools and technologies are allowed, except those clearly mentioned in this document.
6. Although a distributed Microservices architecture is the target - not to be now addressed, the team needs to start
   with a Monolith with loose coupling between components.
7. Security issues in applications previously developed by the company highlight the need for careful design of what can
   have security implications. Thus, the team needs to think about some secure by design principles and threads to
   mitigate. Mention the rules described
   at [SEI. 20189](https://wiki.sei.cmu.edu/confluence/display/java/SEI+CERT+Oracle+Coding+Standard+for+Java) that were
   applied. Proper tools should be used.