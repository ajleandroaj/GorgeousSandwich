# 1 Context

Two years ago, the FoodSoftware developed a new application that made some services available online for
GorgeousSandwich. It proved to be a success and remained active, gaining notoriety.

Some changes have been considered, as explained in the rest of this document. A possible solution to some utilization
peaks has great importance for the application’s success. Thus, a microservice architecture will be explored with a
decomposition of the actual working application. However, the development team is inexperient, and too much complexity
at once is inappropriate. Thus, a prototype will be now developed by incremental migration. It will not have the current
application as a basis, but a previous prototype implemented when the application was first thought.

The prototype presentation must include the build and the execution at the command line, with instructions available in
the repository so that the process can be replicated by others who are not part of the current development team.

Aware of the team’s difficulties, this new prototype does not need to include monitorization/observability, and circuit
breaker, among other features, but local method calls need to be replaced by synchronous remote calls, or better
options. However, service discovery is desirable because it is crucial to enable the growth and development of a
solution based on microservice-oriented architecture.

Direct dependencies in the database are to be eliminated depending on the adopted data management strategies - the use
of messaging needs more time and will be covered in the future.

The architecture design is to be done as part of a process of creating an exploratory prototype: the intention is not to
achieve a releasable or reusable system, but to explore an idea, technology, domain, quality attributes, or others, and
to have something capable of executing and enabling stakeholder feedback.

The appropriate database technologies is an issue that deserves special attention in the development of the prototype
with a detailed analysis in order to support the decision of the appropriate ones for the final application.

Security is again important. The team needs to think about some secure by design principles and threads to mitigate and
the use of some tools that carry out checks at various levels should be considered. Denial of Service and injection, due
to recent problems, must deserve special attention at the software level. The rules
from [SEI. 20189](https://wiki.sei.cmu.edu/confluence/display/java/SEI+CERT+Oracle+Coding+Standard+for+Java) that were
applied need to be described.

# 2 Mode details

## 2.1 More possible architectural drivers

The features that were included in the initial prototype are to be again present in this one and demonstrated, but some
others are to be considered:

1. The prototype to be developed must be accessible using a web browser or a simple application that transfers data to
   and from a server that supports some protocols allowing the use of API, such as [Curl](https://curl.se/)
   and [Postman](https://www.postman.com/).
2. This prototype must be developed in the near six weeks.
3. Authentication is needed.
4. The use of GraphQL is mandatory. At this moment no frontend application is needed but, it will be in the future. A
   GraphQL playground should be used to test queries, analyse the response, and check out the documentation, schema and
   types for the needed fields. It will be needed for the demonstration of the application.
5. Maintainability is a characteristic that the team has to carefully consider during the architecture design.
   Appropriate metrics should be used. At least one is mandatory,
   an [experimental metric](https://blog.hello2morrow.com/2018/12/a-promising-new-metric-to-track-maintainability/) to
   indicate the maintainability level of the application. The Sonargraph-Explorer tool is able to calculate it.
6. Testing on various quality dimensions is mandatory for the prototype, including the business rules captured in domain
   layers and what can ensure the correct functioning of the application and its API. More than the number of tests, it
   is important to explain and document what the tests allow to verify, regardless of the type of tests, together with
   the analysis of the prototype with inclusion in the repository of specific reports of the applications used.
   Contract-based testing must be used whenever two services need to communicate. Tests must be used in the
   demonstration.
7. In addition to what is explicitly stated in the document as mandatory, only opensource tools are allowed.
8. An on-prem solution is desirable with deployment based on containers.
9. The discoverability of new service instances is to be demonstrated.
10. The OpenAPI ([Swagger](https://swagger.io/specification/)) specification must be used for API documentation.

Also, consider some changes/additions in relation to the initial prototype:

1. Sandwich management: Each sandwich has a designation, a selling price, and multiple descriptions, although one per
   authorized language. The language is to be detected by the application. The allowed languages must be changed in a
   few seconds, but without impacting what was previously accepted. However, descriptions should never be removed.
2. Shop management: A shop has a designation, an address, a person in charge, and opening hours that may differ
   depending on the days of the week. The person in charge has a name and an email. A store needs to know the expected
   deliveries for a certain period which can include a few hours a day or even an extended period that can include
   several days, also those already attended and those that have not yet been completed. Each store defines the minimum
   acceptable advance for its deliveries and the maximum number of deliveries for each period of N minutes (from its
   most recent opening, the last period may be less than N). These values can be changed, but not at the code level, and
   never affecting what is already in progress.
3. Order management: A client can register sandwiches and their quantities for delivery on specific day(s)/time(s) and
   shop(s). The total price is informed by the prototype. Please note that a sandwich can never be sold below zero,
   despite the promotions applied.
   The prototype must allow the generation of monthly reports of orders placed.
4. Customer management: A customer has a name, a tax identification number, an address, an email and authentication
   data. The tax identification number and the email are unique and mandatory attributes.
5. Promotions management : Global and local promotions can be specified. While global promotions apply to all stores,
   local promotion does not. Both types specify percentages that apply as a discount to the unit price of specific
   products and have a specified period for their application.
   Promotions are reflected in the price according to different possibilities: only the most favourable or local and
   global promotions are applied cumulatively. Switching from one possibility to another should be made within seconds,
   without affecting what is finished.
6. Surveys management: Customers will be able to give their opinion on various aspects. At this point, only the
   definition of questionnaires matters, not their availability. Each questionnaire must have a number of questions and
   possible answers to be chosen, exclusive or not, by the customers. The prototype must provide questionnaires in PDF
   format.

## 2.2 Documentation

Consistency across all artifacts is valued. The following sections detail what must be documented and how.

### 2.2.1 Before starting or during design

Before starting any iteration design, present all requirements and possible drivers as explained during lectures.

During the design and for all iterations, track the progress. Thus, include Kanban Design Boards and record sketches of
design views and decisions.

Write down the responsibilities allocated to the various elements under consideration when creating the architectural
structures, and include them in each iteration documentation. In summary, during the design process, the following
elements should be captured in a single document:

* The diagram(s) that represent(s)s the structure(s) produced,
* The element responsibilities table,
* The relevant design decisions and their rationales.
  Depending on the iteration purpose, other pieces of information might also be captured, such as:
* A runtime representation of the element´s interaction,
* The initial interface specifications (how elements interact with each other).
  See how the documentation is supposed to be provided and additional details in section 4.2 on page 12.

### 2.2.2 Architecture evaluation, prototype analysis and other issues

**Architecture evaluation**
The design should be rethought, and all architecture approaches for specific quality attribute concerns are to be
re-examined. The team should review the existing and newly discovered risks and tradeoffs and discuss them. A risk is
defined as an architectural decision that may lead to undesirable consequences in light of quality attribute
requirements.
Alternatively, another approach can be used: a tactics-based questionnaire, one for each quality attribute under
consideration. It is considered an "(even lighter) lightweight evaluation method" (L. Bass, P. Clements, and R. Kazman.
2021. Software Architecture in Practice (4 ed.). Addison-Wesley Professional.). It aims to help in the analysis of what
was achieved by the team.
It can be discussed by the team and fulfilled together.
For each question, the following information is to be recorded (L. Bass, P. Clements, and R. Kazman. 2021. Software
Architecture in Practice (4 ed.). Addison-Wesley Professional.):

* The tactic is addressed or not by indicating “Y” in the “Supported” column if the tactic is supported in the
  architecture and “N” otherwise.
* The anticipated/experienced difficulty or risk of implementing the tactic using the (H = high, M = medium, L = low)
  scale - the “Risk” column is to be used for this effect. For instance, a tactic that was of low difficulty or risk to
  implement would be labeled L (or which is anticipated to be of low difficulty, if it has not yet been implemented).
* If it has been used, record how it is realized in the system or will be (e.g., code developed by the team - which code
  modules, frameworks, or packages implement this tactic – or other people in the same organization, generic frameworks,
  or other externally produced components.

The exact design decisions made to realize the tactic and where the realization is visible and can be checked - valuable
for auditing purposes, among others

* Any rationale or assumptions made in the realization of this tactic. In the “Rationale” column, describe the rationale
  for the design decisions made (including a decision to not use this tactic). State the implications of the decision (
  more effort, additional time needed, evolution).
  Two questionnaires are available on Moodle as xlsx files. They can have column widths and row heights easily adjusted.
  No other changes are necessary, although not prohibited. However, they need to be justified with adequate
  bibliographic support.

Other questionnaires may be needed and the following sources can be used: (L. Bass, P. Clements, and R. Kazman. 2021.
Software Architecture in Practice (4 ed.). Addison-Wesley Professional.) and (H. Cervantes and R. Kazman. 2016.
Designing software architectures: a practical approach. Addison-Wesley Professional.), among others.

**Prototype analysis and other issues**
The reports generated by specific tools are valuable documentation to be included in the team repository. They should be
used to support the prototype analysis, which should include what was achieved in the prototype.

In the description of the migration process, refer to what was done in the scope of ADD and mention the evidence of this
process in Bitbucket. It is not enough to show the microservices in their final state, but how they arrive at them,
namely applied standards and principles that can and should be referred to in commit messages with data about the which
is being migrated.

## 2.3 Academic requirements and specifications

Consider that:

* The use of more than one programming language for different microservices is mandatory.
* A context map is to be delineated using the [ContextMapper framework](https://contextmapper.org) and included in the
  ADD documentation. Include the files in the repository.
* The architecture design method to be used is ADD 3.0 (H. Cervantes and R. Kazman. 2016. Designing software
  architectures: a practical approach. Addison-Wesley Professional.). It is mandatory, also a DDD approach.
* Use UML 2.5 notation.
* The distribution of work must ensure that each member has analysis, design, implementation and testing activities,
  with their description included in the report. Each team member should be responsible for at least one microservice.
* Groups with less than five elements are exempt from having survey management in the prototype.

# 3 Terms and conditions

## 3.1 Feedback and guidance

Lab classes should be used to have guidance and feedback. All groups must provide updates on their progress. The
frequent feedback over several lab classes should be used to guide the work, and not just the ones near submission.
Students free from attending classes must comply with all other rules (e.g., deadlines) and provide frequent feedback on
their work as previously agreed with the teachers.
The proposed process includes some of the activities to be carried out. They can be developed in parallel or
sequentially, for example one each week. The team should organize and divide the work as appropriate.

* **Activity 1:**

    * Technological exploration and debate
    * ADD: mainly gathering of all architectural drivers
* **Activity 2:**

    * ADD: mainly identification of bounded contexts in a context map (aggregates, entities, ...) and decomposition
      approaches (business capability and subdomain, but also some principles)
    * Development (work division, start)
* **Activity 3:**

    * Architectural design finalization
    * Architecture evaluation with lightweight ATAM and/or tactics-based questionnaire(s)
    * Development, contract-based and other automatic tests
* **Activity 4:** Mainly for development and more technological explorations for deployment and prototype presentation
* **Activity 5:** Development, contract-based and other automatic tests (conclusion)
* **Activity 6:**

    * Prototype analysis: The prototype was developed with some objectives. Did it allow to understand the consequence
      of some decisions?
    * Report that aggregates everything
    * Remaining tasks

These activities must be documented and the resulting artifacts placed inside the repository (Markdown format) to allow
monitoring of the work whenever necessary or requested. However, the relevant parts must be aggregated in the final
report.
