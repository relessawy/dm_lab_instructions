Agenda
===
* Use Case Overview
* Business Object Model
* Decisions Authoring
* Decisions as a Service
* Testing a Decision Service
* Executing a Decision Service

<!-- # 1 Use Case Review

The Credit Card Dispute process is not straightforward. It involves different actors inside, and outside of, the company. These actors need to have visibility and control at all times into what is happening during the processing of each dispute.

We have identified the actors involved in the overall CC Dispute process. These actors are outside of the corporation and we can think of them as external entities that we will connect with to get information, but that do not participate in the dispute resolution process. We will see more of that in the Case Management Scenario.

One of the requirements to successfully process a dispute is that all of the parties involved are aware of the dispute status at all times, since they can all influence the final resolution of the dispute. Because different parties will have visibility into, and sometimes control over, the process, we need a domain model that clearly depicts the objects that describe the data and the actors that do interact in the resolution of the Credit Card Dispute process.

So far we can identify 3 major external entities:

- `Card Holder`
- `Fraud Data`
- `Additional Informations`

The difference between these entities and the Actors is that these entities are the ones that contains data that is relevant to the processing of the dispute. This data will come from different actors and will be stored in the dispute process instance as process variables. -->

# Use Case Overview

You are a business automation specialist consultant who was hired by a credit card issuer company, Pecunia Corp. Your task now is to automate the decisions and rules of a _Credit Card Dispute_ process.

## Credit Card Dispute - Transaction Risk

Every dispute has a possible risk to be incorrect or to be a fraud. Based on existing data, it is possible to determine the set that determines whether the dispute can be qualified for automated chargeback.

For Pecunia Corp., depending on the customer status and value disputed, the risks can be higher or lower. The bank handled the following rules to you:

- For a **standard** customer, and a dispute amount **between 0 and 100**, the risk is **low**.;
- For a **standard** customer, and a dispute amount **between 100 and 500**, the risk is **medium**;
- For a **standard** customer, and a dispute amount **above 500**, the risk is **high**.
- For a **silver** customer, and a dispute amount **below 250**, the risk is **low**.
- For a **silver** customer, and a dispute amount **between 250 and 500**, the risk is **medium**.
- For a **silver** customer, and a dispute amount **above 500**, the risk is **high**.
- For a **gold** customer, and a dispute amount **below 500**, the risk is **low**.
- For a **gold** customer, and a dispute amount **over 500**, the risk is **medium**.

In your implementation, the business users must have the ability to change criteria anytime if needed, and apply the changes according to the release processes of Pecunia Corp.

You should also consider that the user can change the criteria without technical assistance. Pecunia Corp., business users cannot code, therefore, your solution should allow them to update the rules using standard spreadsheet-like decision tables or quasi natural language.

Make sure you also deliver automated tests for your rules.

# 2 Business Object Model

In this section you will learn

1. What are Business Object Models;
2. How to create Business Object Models inside the Business Central;
3. How to import a project into your space.

## The Business Domain Context

You, as business domain expert, need to define what the domain model is for the business capability you're trying to automate. Eric Evans coined the term [_Domain Driven Design_](https://en.wikipedia.org/wiki/Domain-driven_design) that holds 3 main guiding principles:

- Focus on the core domain.
- Explore models in a creative collaboration of domain practitioners and software practitioners.
- Speak a ubiquitous language within an explicitly bounded context.

So based on this development practice, the first and very important task when automating a core business capability is to create a definition of the business entities within the context of the Credit Card Dispute domain. In our case, our first entity is the `Credit Card Holder`. The definition in the context of our use case maybe totally different from the definition inside other organizations. But within our use case, it will be common definition among the team of business and technology experts, it will be part of our _ubiquitous language_.

## Creating the Card Holder entity

1. To access **Business Central**, go to your OpenShift console tab. Jump to step 2 if you're already logged in. If you're not yet logged in, or have been logged out, log in using these credentials:

  - user: `userX`{{copy}}
  - password: `openshift`{{copy}}

  ![OpenShift Console]({% image_path openshift-console.png %}){:width="600px"}

1. Make sure you are on the `Developer` perspective and that you have the `rhpam-userX` project selected. On the left menu, select the `Topology` option.

1. You will see listed the 3 components: `rhpam7-rhpamcentr`, the `rhpam7-kieserver` and `react-web-app`. From this page, you can already find a link to open Business Central. Click on it to open Business Central in another tab.

  ![PAM Project]({% image_path topology-details.png %}){:width="600px"}

1. Login to Business Central with the credentials u:`pamAdmin`, p:`redhatpam1!`

  ![Business Central Console]({% image_path business-central-console.png %}){:width="600px"}

1. Select _Design_ from the main menu. You will be redirected to your working space. You will see a list of _Spaces_, with a single space named _MySpace_.

1. Click on _MySpace_. This is the sandbox in which you'll define your projects, and within those projects, your projects assets.

1. Since this project is going to be used to deliver a case management implementation, we need to add a new `Case Project`. In order to do so, click on the arrow right next to the _Add Project_ button and select the option `Case Project`.

  ![Business Central Asset CCD Project]({% image_path add-new-case-project.png %}){:width="600px"}

1. When the _Add Project_ wizard opens up, type in

    *  `ccd-project` as the name of the project, and
    * `Assets to Automate credit card dispute process` as the description of your project.

  This project is your business boundary that encapsulates your business capability. Once the creation of your project is complete, you will see it in your space:

  ![Business Central Asset CCD Project]({% image_path business-central-asset-ccd-project.png %}){:width="600px"}

1. Select the `ccd-project`. You should see the following page with the project content.

    ![Business Central Asset Empty Project]({% image_path business-central-asset-empty-project.png %}){:width="600px"}

1. Notice there's a blue button called `Add Asset`.  Click on the `Add Asset` button and you will be presented with a catalog of the wizards to create assets.An asset is a business resource of the project like Rules, Processes, Decision Tables, Data Objects, Data Forms, etc._

    ![Business Central Asset Catalog]({% image_path business-central-asset-catalog.png %}){:width="600px"}

1. Select the wizard for _Data Object_ from the catalog to create your business object model for the _Credit Card Holder_ enter the following and Click OK:

  * type `CreditCardHolder`{{copy}} as the name of the object sandbox
  * select `com.myspace.ccd_project` as the Package.

    ![Business Central CCD Object]({% image_path business-central-CCD-object-new.png %}){:width="600px"}

1. You will see the new created object with no properties, lets click on the `+add field` button to start adding the properties to our CreditCardHolder data object.

    ![Business Central CCD Object New Empty]({% image_path business-central-CCD-object-new-empty.png %}){:width="600px"}

1. In the _New Field_ window, enter the following values and click on _Create_:

    - Id: `age`
    - Label: `Age`
    - Type: `Integer`

    ![Business Central CCD Object New Properties]({% image_path business-central-CCD-object-new-properties.png %}){:width="600px"}

The first step to automate a process or decision is to define and specify the Business Object Model. In this exercise you've created the Entity `CreditCardHolder` and defined it's `age` field. These entities will be used to store the information that you need to make decisions and drive process execution.

## Import Remainder of Project

We learned how easy it is to create a new project and new data objects. Let's now import an existing project with more Business Object Models.

In order to do this, let's delete the `ccd-project` project and learn how to import an existing project in a git repository.

### Deleting the ccd-project project

  1. Delete the current project

  2. At the top of the screen under the main heading, click the _ccd-project_ to bring you back to the homepage for the project

  ![Business Central Breadcrumb bar ccd project]({% image_path business-central-breadcrumb-bar-ccd-project.png %}){:width="600px"}

  2. Delete the project by clicking the hamburger menu & selecting _Delete Project_

  ![Business Central Delete CCD Project]({% image_path business-central-delete-ccd-project.png %}){:width="600px"}

  3. Type in _ccd-project_ and click `Delete Project`
  4. If asked you can `Discard unsaved changes and proceed`.

## Importing a project from external git repository

Let's import the project with all the Data Objects relative to the Domain Model:

  1. Click the `Import Project` button;

  2. On the pop-up, enter [https://github.com/RedHat-Middleware-Workshops/rhpam-rhdm-workshop-v1m2-labs-step-2.git](https://github.com/RedHat-Middleware-Workshops/rhpam-rhdm-workshop-v1m2-labs-step-2.git) as the _Repository URL_ and click `Import`

  3. On the _Import Projects_ screen, select the _ccd-project_ and click `Ok`

  ![Business Central Delete CCD Project]({% image_path business-central-import-ccd-project.png %}){:width="600px"}

  4. Examine the other newly-imported entities

Congratulations, now you that we've seen how to define Data Objects, we can now start working with the automation of our business rules and decisions!

