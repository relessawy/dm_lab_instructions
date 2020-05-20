Agenda
===
* Use Case Overview
* Business Object Model
* Decisions Authoring
* Decisions as a Service
* Testing a Decision Service
* Executing a Decision Service

<!-- # Use Case Review

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

# Lab 1 Business Object Model

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

# Lab 2 Authoring Decisions and Rules

In this section you will learn:

1. How to use your Business Object Model to automate decisions and policies.
2. The different types of authoring tools available to you.
3. How to use your automated decisions and rules.

## Intro

Solving a credit card dispute depends on several variables, like:

- The type of customer
- The amount of the dispute

The knowledge of how to apply these rules and decisions is tacit and lives only in the head of other domain experts. In order to automate the process, you will first have to express the business policies that determine how a dispute is handled in the form of rules.

For this particular case, 2 sets of rules are defined for different stages on the process:

## Calculating the Risk

At the moment, all processing is manual. There is a group of agents dedicated to making decisions based on the data of the dispute. This is not only expensive, but also very prone to errors and inconsistencies.

The cost of processing a dispute for Pecunia Corp. is high and independent of the amount that is being disputed. That is why it's very important to have flexible rules that reduce the processing cost and time. Apart from reducing cost, this will also improve customer experience.

The rules defined for the process are:

- Automatic chargeback is only available to Platinum and Gold Credit Card Holders

- The risk of the transaction is determined by the type of user and the amount of the dispute

      - Standard customer 0-100: low risk
      - Standard customer 100-500: medium risk
      - Standard customer above 500: high risk
      - Silver customer 0-250: low risk
      - Silver customer 250-500: medium risk
      - Silver customer above 500: high risk
      - Gold customer 0-500: low risk
      - Gold customer above 500: medium risk

- If the customer's billing address is in the state of Texas, California or Florida, the dispute should be considered of higher risk.

## The Authoring Tools

You have defined the Business Object Model in the previous lab. In the last step of the previous lab, you've imported a project with the complete Business Object Model. You will use this project as the base for this lab.

### Decision Tables

A very common way to define the logic behind risk assessment is to store this information in spreadsheets. With Red Hat Process Automation Manager you can use the same spreadsheet approach and make it an executable asset (i.e. a set of rules) in the engine. In this section you are going to create a _Decision Table_ to automate the risk assessment rules that were given to you.

1. First, go back to the Library view and click on the blue `Add Asset` button in the top right corner.

    ![Business Central Decision Table Add Asset]({% image_path business-central-decision-table-add-asset.png %}){:width="600px"}

2. Select `Guided Decision Table` from the catalog of assets (the UI allows you to filter the assets per type by using the filter drop-down and input box in the upper-left of the screen. Select `Decisions` to filter on decision assets).

    ![Business Central Decision Table Add Asset Guided]({% image_path business-central-decision-table-add-asset-guided.png %}){:width="600px"}

3. Type the following values on the `Create New Guided Decision Table` wizard

    - Guided Decision Table (Name): `risk-evaluation`
    - Package: `com.myspace.ccd_project`

    Click _Ok_ and _Finish_.

4. You should see the `Guided Decision Table` editor with an empty table.

    ![Business Central Decision Table New]({% image_path business-central-decision-table-new.png %}){:width="600px"}

    There are 5 tabs in the editor:

    - _Model_: The model diagram of the Decision Table
    - _Columns_: Add, Edit or Delete columns in your table. A column can represent an Attribute, Metadata, a Constraint (rule left-hand-side or LHS) on a property of a Business Model Object, or an Action (rule right-hand-side or RHS).
    - _Overview_: Contains the meta-information of your asset: Version, Description, Last Modified, etc.
    - _Source_: Is the actual source code that is generated from the Decision Table Model. In the runtime engine, decision tables are translated into native DRL (Drools Rule Language), where each row in the table is translated into a rule.
    - _Data Objects_: Lists the Business Objects available to the editor to be used as conditions and/or actions.

In our system, the properties evaluated to determine the risk scoring are:

  - Status of the Credit Card Holder
  - Total Amount disputed from the Fraud Data

Let's add the Credit Card Holder condition column.

5. Go to the _Columns_ tab and click on the button `Insert Column`, select `Add Condition` and click Next.

    ![Business Central Add Condition]({% image_path business-central-add-condition.png %}){:width="600px"}

6. You need to define which object is going to be evaluated. Click on `Create new Fact Pattern`. Select `CreditCardHolder` as the _Fact type_ and define a variable called `holder` as the _Binding_. Click _Ok_ to dismiss the fact pattern dialog, then click _Next_.

    ![Business Central Create Pattern]({% image_path business-central-create-pattern.png %}){:width="600px"}

7. The calculation type is the type of evaluation that you are going to apply. In this case it will be against literal values. Select `Literal value` and click _Next_.

8. You need to define a constraint on the card holder's status, so select the `status` field and click _Next_.

    ![Business Central Create Pattern Field]({% image_path business-central-create-pattern-field.png %}){:width="600px"}

9. Next, select the operator for the constraint. Select `equal to` from the drop down menu and click _Next_.

    ![Business Central Create Pattern Field Operator]({% image_path business-central-create-pattern-field-operator.png %}){:width="600px"}

10. Since there are only 3 possible statuses, you are going to configure the fields with the values below and then click _Next_.

    - the _Value list_ with the following values: `Standard,Silver,Gold`
    - The _Default value_ to `Standard`

    ![Business Central Create Pattern Field Values]({% image_path business-central-create-pattern-field-values.png %}){:width="600px"}

11. You can now configure the header of the column `Header`: `Status`.

    ![Business Central Create Pattern Field Header]({% image_path business-central-create-pattern-field-header.png %}){:width="600px"}

12. Click Finish and go back to the `Model` tab in the editor. You should see the newly created column.

13. Repeat the same steps to add 2 more columns:

    - Pattern:
        - Fact type: `FraudData`
        - Binding: `data`
    - Calculation Type: `Literal value`
    - property: `totalFraudAmount`
    - operation:
        - `greater than or equal to` for one column. Use header name: "Minimum Amount"
        - `less than` for the second column. Use header nam "Maximum Amount"

    Note that for the second column you don't need to create a new fact pattern, you can reuse the existing one.

    At the end your decision table should look like this:

    ![Business Central Decision Table Columns]({% image_path business-central-decision-table-columns.png %}){:width="600px"}

13. Click on the _Save_ button to save the decision table.

14. Next go to the `Columns` tab and Click on `Insert Column`. This time you add an Action, the Right-Hand-Side of a rule. This action will be fired when the conditions are met. Select `Set the value of a field` and click next.

15. Set the risk scoring property of the `FraudData` object. So in the dropdown menu select the object `FraudData` bound to the variable `data`.Click Next.

    ![Business Central Decision Table Columns Action Data]({% image_path business-central-decision-table-columns-action-data.png %}){:width="600px"}

16. Select the field `disputeRiskRating` and click Next. You don't have a list of values so click Next. Type `Risk Scoring` as the header for the column and click Finish.

    ![Business Central Decision Table Columns Action Data Finish]({% image_path business-central-decision-table-columns-action-data-finish.png %}){:width="600px"}

17. Click on the _Save_ button to save the decision table.

18. Go back to your `Model` tab. You are now going to add the actual constraints and actions, i.e. the actual rules. Looking at our requirements, the first constraint is defined as:

  _For a standard customer, and a dispute amount between 0 and 100, the risk is low._

  There are 4 levels of risk: low, medium, high and very-high. You will define these risk-levels as integers: 1,2,3, and 4.

19. Click on the button Insert and select append row from the dropdown menu.

     ![Business Central Decision Table Columns Action Data Finish Model]({% image_path business-central-decision-table-append-row.png %}){:width="600px"}

20. Click on the _Description_ cell of the new row and type "_Standard customer low risk_". Use the following values for the other columns:

     - Description:`Standard customer low risk`{{copy}}  
     - Status:`Standard`{{copy}}  
     - Minimum Amount:`0`{{copy}}  
     - Max Amount:`100`{{copy}}  
     - Risk Scoring:`0`{{copy}}

    Your decision table should look like this. Click Save.

    ![Business Central Decision Table First Row]({% image_path business-central-decision-table-first-row.png %}){:width="600px"}

21. Based on the business rules, apply the same procedure for the rest of it.

  - Standard customer 0-100: low risk (Risk scoring = 0)
  - Standard customer 100-500: medium risk (Risk scoring = 1)
  - Standard customer above 500: high risk (Risk scoring = 2)
  - Silver customer 0-250: low risk (Risk scoring = 0)
  - Silver customer 250-500: medium risk (Risk scoring = 1)
  - Silver customer above 500: high risk (Risk scoring = 2)
  - Gold customer 0-500: low risk (Risk scoring = 0)
  - Gold customer above 500: medium risk (Risk scoring = 1)


  At the end your decision table should look as follows:

  ![Business Central Decision Complete]({% image_path business-central-decision-table-complete.png %}){:width="600px"}

20. Save the table once you finish.


## Guided Rules

Guided Rules are another type of rules that you can create in Business Central. Once you have defined the Business Object Model, you can create rules that check conditions on the properties of these objects, rules that define conditions on combinations of objects, etc. For example, you can define a rule with a constraint on a Credit Card Holder's age, his/her status, riskRating, etc. If the condition or conditions are met, the rule is set to be _matched_ and becomes eligible for _firing_. When the rule fires, it executes the action defined in the rule. The action is the _THEN_ part of the rule, or what is also called the rule's consequence, or Right-Hand-Side (RHS).

In the previous section you have created the rules, in the form a decision table, that determine the credit risk scoring. In this section, you create the rules that determine whether the credit card dispute is eligible for automatic chargeback. You will create this rule in the form of _Guided Rule_.

In the case of the rules for automatic chargeback you are evaluating only the Credit Card Holder. So let's create the rule.

First, tell the rule what object or collection of objects is going to be evaluated. Rules have a very basic syntax, and basically consist of 3 parts:

- the _When_ part defines the constraints of the rule. I.e. these are the discrimination criteria or conditions which, if they are met, cause the rule to fire.
- the _Then_ part defines the actions the rule will execute when it fires. This can be for example setting specific data on a fact (in the Business Object Model), but this can also be inserting new, inferred, data into the rules engine. For example, based on a card holder's age, you can infer that he/she is an adult.
- the _properties_ or _attributes_ part. Here you set additional characteristics of the rule, for example the group of rules it belongs to.

To create the rule:

1. At the top of the screen under the main heading, click the _ccd-project_ to bring you back to the homepage for the project

    ![Business Central Breadcrumb bar ccd project]({% image_path business-central-breadcrumb-bar-ccd-project.png %}){:width="600px"}

2. Click on the blue button `Add Asset` on the right upper corner of the Library View.

    ![Business Central CCD BOM Project]({% image_path business-central-ccd-bom-project.png %}){:width="600px"}

3. In the _Add Asset_ screen, select _Decision_ from the drop-down filter menu to filter on decision assets.

    ![Business Central Add Assets Filter]({% image_path business-central-add-assets-filter.png %}){:width="600px"}

4. Select `Guided Rule` from the filtered catalog of Wizards.

5. Set the following data in the creation wizard:

    - Name: `automated-chargeback`
    - Package: `com.myspace.ccd_project`

    ![Business Central Guided Rule New]({% image_path business-central-guided-rule-new.png %}){:width="600px"}

6. Click ok. You should see a banner in green telling you that the asset was success fully created. The UI will display the editor that allows you to author your rule.

    ![Business Central Guided Rule New Wizard]({% image_path business-central-guided-rule-new-wizard.png %}){:width="600px"}

7. You will see 4 tabs in the editor panel. Select the tab that says "Data Objects"

    ![Business Central Guided Rule Import Data Object]({% image_path business-central-guided-rule-import-data-object.png %}){:width="600px"}

8. You should see 4 items listed: `AdditionalInformation`, `CreditCardHolder`, `FraudData`, and `Number`. These are shown by default as the rule is created in the same folder/package as these data objects. If `CreditCardHolder` is not listed, click on the blue _New Item_ button to import it.

    ![Business Central Guided Rule Import Data Object New]({% image_path business-central-guided-rule-import-data-object-new.png %}){:width="600px"}

9. Return to the _Model_ tab and Click on the green plus-sign to the right of the word _WHEN_.

    ![Business Central Guided Rule New Fact]({% image_path business-central-guided-rule-new-fact.png %}){:width="600px"}

10. Select the object `CreditCardHolder`, and click ok. You are now telling the rule engine that every time there is a CreditCardHolder this rule needs to be activated.

    ![Business Central Guided Rule New Fact Select]({% image_path business-central-guided-rule-new-fact-select.png %}){:width="600px"}

    In order to match the criteria of the functional requirement, you need to add a restriction on one of the card holder's properties. Automated chargeback is only approved for CC Holders that have the `status` _Gold_ or _Platinum_.

11. Click on the condition `There is a Credit Card Holder`, a new wizard will open. You now _Add a restriction on a field_. From the dropdown box select the `status` field of the CC Holder.

    ![Business Central Guided Rule New Property Select]({% image_path business-central-guided-rule-new-property-select.png %}){:width="600px"}

12. From the dropdown box select that the status `is contained in the (comma separated) list`. Click on the pencil icon and add the literal values  _Gold_ and _Platinum_, separated by a comma. TIP: You can also add enumerations containing these values to have them pre-populated for you. Click on the _Save_ button to save the asset.

    ![Business Central Guided Rule New Property Select Values]({% image_path business-central-guided-rule-new-property-select-values.png %}){:width="600px"}

13. Go back to the _Data Objects_ tab. If the `FraudData` data object has not been imported yet, complete the same procedure, to import it. Go back to the _Model_ tab and add a constraint on the `FraudData` object the same way as you did before. You don't need to put a constraint on any property of the `FraudData`. Instead, you just need to make sure that it's there.

    ![Business Central Guided Rule Check Fraud Data]({% image_path business-central-guided-rule-check-fraud-data.png %}){:width="600px"}

14. When you want to modify the data in the objects of the Business Model or facts, you need to be able to reference the matched object from within the rule. To allow this, the object needs to be bound to a variable inside the rule. This makes the object accessible in both the left-hand-side (LHS) and  right-hand-side (RHS) through the variable. Click on the fact declaration `There is FraudData`, the wizard to modify the constraints will open.

15. In the _Variable name_ field at the bottom of the form, type `data` as the name of the variable that you want to bind the `FraudData` object to. Click on the _Set_ button. Save the asset.

    ![Business Central Guided Rule Modify Fraud Data]({% image_path business-central-guided-rule-modify-fraud-data.png %}){:width="600px"}

  Now set the property of automated chargeback to true on the `FraudData` object, so the dispute can be processed accordingly. Since this is the decision you are making, and thus the _action_ of the rule, you define this as the THEN clause,  also known as the Right Hand Side (RHS) or Action section of our rule.

  All of the information of the CC dispute is stored in facts. These facts can live in a session that the engine will keep in memory. So every time you evaluate a new fact, or change something to an existing fact, you will have all of the Objects in the session available in the process of decision making. In the RHS, or action, part of the rule you can change the values of any property on the objects that you can reference via the variables, or even create and add new objects/facts to the session (this is usually referred to as _inferring_ new data or information). Every time a property in an object changes, all of the decisions in which this property is used will be reevaluated to make sure that no other rule needs to be applied.

1. Click on the green plus-sign next to the _THEN_ keyword.

  ![Business Central Guided Rule New Then Condition]({% image_path business-central-guided-rule-new-then-condition.png %}){:width="600px"}

2. When the `Add new action` wizard opens select `Change field values of data` and click on _OK_. This will automatically select the `FraudData` object, as this is the only object that has been bound to a variable.

  ![Business Central Guided Rule Modify Fraud Data Wizard]({% image_path business-central-guided-rule-modify-fraud-data-wizard.png %}){:width="600px"}

3. Now set the value of the property `automated` to `true`, indicating that an automatic chargeback applies. Click on the action `Set value of FraudData [data]` and select the field `automated`. Click on the pencil icon to the right and assign a literal value to the property.

    ![Business Central Guided Rule Modify Fraud Automated]({% image_path business-central-guided-rule-modify-fraud-automated.png %}){:width="600px"}

4. Select `true` as the value for the automated property (this is the default value for booleans, so the property is probably already set to `true`). Note that since the type of data is `boolean`, you can only choose between `true` and `false`.

    ![Business Central Guided Rule Modify Fraud Automated True]({% image_path business-central-guided-rule-modify-fraud-automated-true.png %}){:width="600px"}

5. To validate that everything is correct, click on the _Validate_ button on the top navigation bar and you should see a green "Item successfully validated!" message.

    ![Business Central Guided Rule Validate]({% image_path business-central-guided-rule-validate.png %}){:width="600px"}

6.  Finally, click on _Save_ to save the rule.

You have created your first Business Rule using the Guided editor

# Lab 3 Decision Services

In this section you will learn:

1. How to use the rules that you authored in the previous step;
2. How can you expose your decisions as a service;
3. How to use your automated decisions and rules.

In previous labs we have defined the Business Object Model and the rules and decisions that operate on the model. If you've completed the labs in the previous steps, you can use your existing project.

NOTE: _If you'd prefer to start off fresh you can delete your `ccd-project` project and re-import it using this URL: [https://github.com/RedHat-Middleware-Workshops/rhpam-rhdm-workshop-v1m2-labs-step-3.git](https://github.com/RedHat-Middleware-Workshops/rhpam-rhdm-workshop-v1m2-labs-step-3.git)._

Let's start by having an overview of assets versioning with Business Central.

## Assets Versioning

As you can notice on the diagram below, Red Hat Process Automation Manager is a modular platform to develop and run decisions and processes. Until now we have mostly used **Business Central** workbench to develop rules and business models. All the assets that you create or alter using Business Central are versioned in a repository (a Git-based version control system).

![RHPAM 7 Architecture]({% image_path rhpam-7-architecture.png %}){:width="600px"}

Business Central automatically versions our code within git-based repository, and it also provides a way for us to check the changes and rollback to previous versions necessary.

1. Go to your project library view and select the automated-chargeback rule. Once the editor opens click on the button Latest Version. (**NOTE:** If you re-imported the project then there is probably only 1 version listed).

  ![Business Central Chargeback Versions]({% image_path business-central-chargeback-versions.png %}){:width="600px"}

2. There is also more metadata about your assets stored in the repository, like the name of the user that created the asset, the time and data when it was last modified, etc.

  ![Business Central Chargeback Versions Detail]({% image_path business-central-chargeback-versions-detail.png %}){:width="600px"}

3. Click on any of the versions in the dropdown box and you will see the version of the asset tagged.

  ![Business Central Chargeback Version]({% image_path business-central-chargeback-version.png %}){:width="600px"}

## Understanding the deployment process

After your project is developed, you can build the project in Business Central and deploy it to a configured Process Server. The Process Server is the engine that executes rules and processes. It also allows distributed execution so if, for example, the execution of the case rules will be performed at bank branches, you can deploy as many Process Servers as you need all connected to your Business Central Authoring console.

The Process Server supports the capabilities configured in its server configuration. A server configuration is the template that defines the configuration of a group of Execution Servers (a group can contain zero or more execution servers).

There are 2 things that you can configure through the template:

  - Capabilities: What can you execute in your Process Server (Process, Decisions, Planner rules). They are not mutually exclusive. I.e. an execution server can be enabled with zero or more of these capabilities enabled.
  - Deployment Unit: what package of assets (project, Knowledge JAR) you want to deploy on the server to make available for execution.


Let's have a glance of what happens under the covers during a deployment in a managed Process Server:

1. When you _Build and Deploy_ a project in Business Central, the _Deployment Unit_ (KJAR) is created and pushed to the artifact repository (Maven);
2. After this, a deployment request is sent to the Execution Server;
3. The managed Execution Server receives a deployment request for a specific _Deployment Unit_;
4. The Execution Server fetches the _Deployment Unit_ and tries to initialize it.

Once this tasks are completed, you can start, stop, or remove deployment units using Business Central as needed. You can also create additional _Deployment Units_ from previously built projects and start them on existing or new Process Servers configured in Business Central.

## Deploying your project

Let's check your Process Server using Business Central.

1. Return to the Home screen of the Business Central workbench (by clicking on the _Home_ icon in the upper left screen).

2. Click on _Deploy_. This will open the _Server Configurations_ perspective. Notice which capabilities you have enabled for your Process Server.

![Business Central Process Server Server Configurations]({% image_path business-central-server-configuration.png %}){:width="600px"}

2. Return to the Home screen and select _Design_. Select `MySpace`, next, select your Credit Card Dispute project (`ccd-project`). The Library view should open with a list of all your assets. These assets will be compiled and packaged inside a _KJAR_, a _Deployment Unit_.

3. Click on the _Deploy_ button in the top right corner.

    ![Business Central Deploy]({% image_path business-central-deploy.png %}){:width="600px"}

You will see that the project is first built, meaning the assets are compiled and packaged, and then deployed to a Execution Server container. Go back to the Home screen and elect Deploy. You will now see a container running with your newly created decisions.

## Execution

Let's check if the service you deployed is available.

1. Go to the Main Menu and select `Deploy`>`Execution Services`.

2. Click on the URL of the container, and a new tab should open:

  ![Business Central Execution Services Detail]({% image_path business-central-execution-services-detail.png %}){:width="600px"}

3. You may also be prompted for credentials. Use the same credentials you used to log into the Business Central console.
  - user: `pamAdmin`
  - password: `redhatpam1!`

![Business Central Execution Services Info]({% image_path business-central-execution-services-info.png %}){:width="600px"}

4. Notice the Process Service responds with details about the `Kie Container` where your `Deployment Unit` is running.

Congratulations! Now that you have deployed your first business application within the engine, let's learn how about how to automate tests of the rules you created in the Credit Card Dispute project.

