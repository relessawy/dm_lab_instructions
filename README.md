Agenda
===
* Use Case Overview
* [Lab 1 Business Object Model](https://github.com/relessawy/dm_lab_instructions/blob/master/README.md#lab-1-business-object-model)
* [Lab 2 Decisions Authoring](https://github.com/relessawy/dm_lab_instructions/blob/master/README.md#lab-2-authoring-decisions-and-rules)
* [Lab 3 Decisions as a Service](https://github.com/relessawy/dm_lab_instructions/blob/master/README.md#lab-3-decision-services)
* [Lab 4 Testing a Decision Service](https://github.com/relessawy/dm_lab_instructions/blob/master/README.md#lab-4-testing-your-decision-service)
* [Lab 5 Decision Model & Notation](https://github.com/relessawy/dm_lab_instructions/blob/master/README.md#lab-5-decision-model--notation)

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

1. To access **Business Central**, go to your [OpenShift Console], log in using these credentials:

  - user: `userX`
  - password: `openshift`

![OpenShift Console](https://github.com/relessawy/dm_lab_instructions/blob/master/images/openshift-console.png)
 
1. Make sure you are on the `Developer` perspective and that you have the `rhpam-userX` project selected. On the left menu, select the `Topology` option.

1. You will see listed the 3 components: `rhpam7-rhpamcentr`, the `rhpam7-kieserver` and `react-web-app`. From this page, you can already find a link to open Business Central. Click on it to open Business Central in another tab.

  ![PAM Project](https://github.com/relessawy/dm_lab_instructions/blob/master/images/topology-details.png)

1. Login to Business Central with the credentials u:`pamAdmin`, p:`redhatpam1!`

  ![Business Central Console](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-console.png)

1. Select _Design_ from the main menu. You will be redirected to your working space. You will see a list of _Spaces_, with a single space named _MySpace_.

1. Click on _MySpace_. This is the sandbox in which you'll define your projects, and within those projects, your projects assets.

1. Since this project is going to be used to deliver a case management implementation, we need to add a new `Case Project`. In order to do so, click on the arrow right next to the _Add Project_ button and select the option `Case Project`.

  ![Business Central Asset CCD Project](https://github.com/relessawy/dm_lab_instructions/blob/master/images/add-new-case-project.png)

1. When the _Add Project_ wizard opens up, type in

    *  `ccd-project` as the name of the project, and
    * `Assets to Automate credit card dispute process` as the description of your project.
    
2. Click the Add button to create the project

  This project is your business boundary that encapsulates your business capability. Once the creation of your project is complete, you will see the project Asset view. 

![Business Central Asset Empty Project](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-asset-empty-project.png)

1. Notice there's a blue button called `Add Asset`.  Click on the `Add Asset` button and you will be presented with a catalog of the wizards to create assets.An asset is a business resource of the project like Rules, Processes, Decision Tables, Data Objects, Data Forms, etc._

    ![Business Central Asset Catalog](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-asset-catalog.png)

1. Select the wizard for _Data Object_ from the catalog to create your business object model for the _Credit Card Holder_ enter the following and Click OK:

  * type `CreditCardHolder` as the name of the object sandbox
  * select `com.myspace.ccd_project` as the Package.

    ![Business Central CCD Object](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-CCD-object-new.png)

1. You will see the new created object with no properties, lets click on the `+add field` button to start adding the properties to our CreditCardHolder data object.

    ![Business Central CCD Object New Empty](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-CCD-object-new-empty.png)

1. In the _New Field_ window, enter the following values and click on _Create_:

    - Id: `age`
    - Label: `Age`
    - Type: `Integer`

    ![Business Central CCD Object New Properties](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-CCD-object-new-properties.png)

The first step to automate a process or decision is to define and specify the Business Object Model. In this exercise you've created the Entity `CreditCardHolder` and defined it's `age` field. These entities will be used to store the information that you need to make decisions and drive process execution.

## Import Remainder of Project

We learned how easy it is to create a new project and new data objects. Let's now import an existing project with more Business Object Models.

In order to do this, let's delete the `ccd-project` project and learn how to import an existing project in a git repository.

### Deleting the ccd-project project

  1. Delete the current project

  2. At the top of the screen under the main heading, click the _ccd-project_ to bring you back to the homepage for the project

  ![Business Central Breadcrumb bar ccd project](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-breadcrumb-bar-ccd-project.png)

  2. Delete the project by clicking the hamburger menu & selecting _Delete Project_

  ![Business Central Delete CCD Project](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-delete-ccd-project.png)

  3. Type in _ccd-project_ and click `Delete Project`
  4. If asked you can `Discard unsaved changes and proceed`.

## Importing a project from external git repository

Let's import the project with all the Data Objects relative to the Domain Model:

  1. Click the `Import Project` button;

  2. On the pop-up, enter [https://github.com/relessawy/rhpam-rhdm-workshop-v1m2-labs-step-2](https://github.com/RedHat-Middleware-Workshops/rhpam-rhdm-workshop-v1m2-labs-step-2.git) as the _Repository URL_ and click `Import`

  3. On the _Import Projects_ screen, select the _ccd-project_ and click `Ok`

  ![Business Central Delete CCD Project](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-import-ccd-project.png)

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

    ![Business Central Decision Table Add Asset](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-decision-table-add-asset.png)

2. Select `Guided Decision Table` from the catalog of assets (the UI allows you to filter the assets per type by using the filter drop-down and input box in the upper-left of the screen. Select `Decisions` to filter on decision assets).

    ![Business Central Decision Table Add Asset Guided](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-decision-table-add-asset-guided.png)

3. Type the following values on the `Create New Guided Decision Table` wizard

    - Guided Decision Table (Name): `risk-evaluation`
    - Package: `com.myspace.ccd_project`

    Click _Ok_ and _Finish_.

4. You should see the `Guided Decision Table` editor with an empty table.

    ![Business Central Decision Table New](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-decision-table-new.png)

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

    ![Business Central Add Condition](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-add-condition.png)

6. You need to define which object is going to be evaluated. Click on `Create new Fact Pattern`. Select `CreditCardHolder` as the _Fact type_ and define a variable called `holder` as the _Binding_. Click _Ok_ to dismiss the fact pattern dialog, then click _Next_.

    ![Business Central Create Pattern](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-create-pattern.png)

7. The calculation type is the type of evaluation that you are going to apply. In this case it will be against literal values. Select `Literal value` and click _Next_.

8. You need to define a constraint on the card holder's status, so select the `status` field and click _Next_.

    ![Business Central Create Pattern Field](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-create-pattern-field.png)

9. Next, select the operator for the constraint. Select `equal to` from the drop down menu and click _Next_.

    ![Business Central Create Pattern Field Operator](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-create-pattern-field-operator.png)

10. Since there are only 3 possible statuses, you are going to configure the fields with the values below and then click _Next_.

    - the _Value list_ with the following values: `Standard,Silver,Gold`
    - The _Default value_ to `Standard`

    ![Business Central Create Pattern Field Values](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-create-pattern-field-values.png)

11. You can now configure the header of the column `Header`: `Status`.

    ![Business Central Create Pattern Field Header](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-create-pattern-field-header.png)

12. Click Finish and go back to the `Model` tab in the editor. You should see the newly created column.

13. Now lets create the "Minimum Amount" column, Ggo to the _Columns_ tab and click on the button `Insert Column`, select `Add Condition` and click Next.

14. Click on `Create new Fact Pattern`. Select `FraudData` as the _Fact type_ and define a variable called `data` as the _Binding_. Click _Ok_ to dismiss the fact pattern dialog, then click _Next_.

15. Set the Calculation type to `Literal value` and click _Next_.

16. Select the `totalFraudAmount` field and click _Next_.

17. Next, select the operator for the constraint. Select `greater than or equal to` from the drop down menu and click _Next_.

18. Leave the value list empty and click _Next_.

19. Set the column header to "Minimum Amount" and click _Finish_.

20. Repeat steps 13 to 19 to create the "Maximum Amount" column. The only differences are 
    - You don't need to create a new Fact Pattern, just re-use "Fraud Data" (Step 14)
    - The Operation will be `less than` (Step 17)
    - The header will be "Maximum Amount" (Step 19)

    At the end your decision table should look like this:

    ![Business Central Decision Table Columns](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-decision-table-columns.png)

21. Click on the _Save_ button to save the decision table.

22. Next go to the `Columns` tab and Click on `Insert Column`. This time you add an Action, the Right-Hand-Side of a rule. This action will be fired when the conditions are met. Select `Set the value of a field` and click next.

23. Set the risk scoring property of the `FraudData` object. So in the dropdown menu select the object `FraudData` bound to the variable `data`.Click Next.

    ![Business Central Decision Table Columns Action Data](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-decision-table-columns-action-data.png)

24. Select the field `disputeRiskRating` and click Next. You don't have a list of values so click Next. Type `Risk Scoring` as the header for the column and click Finish.

    ![Business Central Decision Table Columns Action Data Finish](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-decision-table-columns-action-data-finish.png)

25. Click on the _Save_ button to save the decision table.

26. Go back to your `Model` tab. You are now going to add the actual constraints and actions, i.e. the actual rules. Looking at our requirements, the first constraint is defined as:

  _For a standard customer, and a dispute amount between 0 and 100, the risk is low._

  There are 4 levels of risk: low, medium, high and very-high. You will define these risk-levels as integers: 1,2,3, and 4.

27. Click on the button Insert and select append row from the dropdown menu.

     ![Business Central Decision Table Columns Action Data Finish Model](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-decision-table-append-row.png)

28. Click on the _Description_ cell of the new row and type "_Standard customer low risk_". Use the following values for the other columns:

     - Description:`Standard customer low risk` 
     - Status:`Standard` 
     - Minimum Amount:`0`  
     - Max Amount:`100` 
     - Risk Scoring:`0`

    Your decision table should look like this. Click Save.

    ![Business Central Decision Table First Row](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-decision-table-first-row.png)

29. Based on the business rules, apply the same procedure for the rest of it.

  - Standard customer 0-100: low risk (Risk scoring = 0)
  - Standard customer 100-500: medium risk (Risk scoring = 1)
  - Standard customer above 500: high risk (Risk scoring = 2)
  - Silver customer 0-250: low risk (Risk scoring = 0)
  - Silver customer 250-500: medium risk (Risk scoring = 1)
  - Silver customer above 500: high risk (Risk scoring = 2)
  - Gold customer 0-500: low risk (Risk scoring = 0)
  - Gold customer above 500: medium risk (Risk scoring = 1)


  At the end your decision table should look as follows:

  ![Business Central Decision Complete](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-decision-table-complete.png)

30. Save the table once you finish.


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

    ![Business Central Breadcrumb bar ccd project](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-breadcrumb-bar-ccd-project.png)

2. Click on the blue button `Add Asset` on the right upper corner of the Library View.

    ![Business Central CCD BOM Project](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-ccd-bom-project.png)

3. In the _Add Asset_ screen, select _Decision_ from the drop-down filter menu to filter on decision assets.

    ![Business Central Add Assets Filter](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-add-assets-filter.png)

4. Select `Guided Rule` from the filtered catalog of Wizards.

5. Set the following data in the creation wizard:

    - Name: `automated-chargeback`
    - Package: `com.myspace.ccd_project`

    ![Business Central Guided Rule New](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-guided-rule-new.png)

6. Click ok. You should see a banner in green telling you that the asset was success fully created. The UI will display the editor that allows you to author your rule.

    ![Business Central Guided Rule New Wizard](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-guided-rule-new-wizard.png)

7. You will see 4 tabs in the editor panel. Select the tab that says "Data Objects"

    ![Business Central Guided Rule Import Data Object](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-guided-rule-import-data-object.png)

8. You should see 4 items listed: `AdditionalInformation`, `CreditCardHolder`, `FraudData`, and `Number`. These are shown by default as the rule is created in the same folder/package as these data objects. If `CreditCardHolder` is not listed, click on the blue _New Item_ button to import it.

    ![Business Central Guided Rule Import Data Object New](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-guided-rule-import-data-object-new.png)
9. Return to the _Model_ tab and Click on the green plus-sign to the right of the word _WHEN_.

    ![Business Central Guided Rule New Fact](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-guided-rule-new-fact.png)

10. Select the object `CreditCardHolder`, and click ok. You are now telling the rule engine that every time there is a CreditCardHolder this rule needs to be activated.

    ![Business Central Guided Rule New Fact Select](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-guided-rule-new-fact-select.png)

    In order to match the criteria of the functional requirement, you need to add a restriction on one of the card holder's properties. Automated chargeback is only approved for CC Holders that have the `status` _Gold_ or _Platinum_.

11. Click on the condition `There is a Credit Card Holder`, a new wizard will open. You now _Add a restriction on a field_. From the dropdown box select the `status` field of the CC Holder.

    ![Business Central Guided Rule New Property Select](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-guided-rule-new-property-select.png)

12. From the dropdown box select that the status `is contained in the (comma separated) list`. Click on the pencil icon and add the literal values  _Gold_ and _Platinum_, separated by a comma. TIP: You can also add enumerations containing these values to have them pre-populated for you. Click on the _Save_ button to save the asset.

![Business Central Guided Rule New Property Select Values](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-guided-rule-new-property-select-values.png)

13. Go back to the _Data Objects_ tab. If the `FraudData` data object has not been imported yet, complete the same procedure, to import it. Go back to the _Model_ tab and add a constraint on the `FraudData` object the same way as you did before. You don't need to put a constraint on any property of the `FraudData`. Instead, you just need to make sure that it's there.

    ![Business Central Guided Rule Check Fraud Data](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-guided-rule-check-fraud-data.png)

14. When you want to modify the data in the objects of the Business Model or facts, you need to be able to reference the matched object from within the rule. To allow this, the object needs to be bound to a variable inside the rule. This makes the object accessible in both the left-hand-side (LHS) and  right-hand-side (RHS) through the variable. Click on the fact declaration `There is FraudData`, the wizard to modify the constraints will open.

15. In the _Variable name_ field at the bottom of the form, type `data` as the name of the variable that you want to bind the `FraudData` object to. Click on the _Set_ button. Save the asset.

    ![Business Central Guided Rule Modify Fraud Data](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-guided-rule-modify-fraud-data.png)

  Now set the property of automated chargeback to true on the `FraudData` object, so the dispute can be processed accordingly. Since this is the decision you are making, and thus the _action_ of the rule, you define this as the THEN clause,  also known as the Right Hand Side (RHS) or Action section of our rule.

  All of the information of the CC dispute is stored in facts. These facts can live in a session that the engine will keep in memory. So every time you evaluate a new fact, or change something to an existing fact, you will have all of the Objects in the session available in the process of decision making. In the RHS, or action, part of the rule you can change the values of any property on the objects that you can reference via the variables, or even create and add new objects/facts to the session (this is usually referred to as _inferring_ new data or information). Every time a property in an object changes, all of the decisions in which this property is used will be reevaluated to make sure that no other rule needs to be applied.

1. Click on the green plus-sign next to the _THEN_ keyword.

  ![Business Central Guided Rule New Then Condition](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-guided-rule-new-then-condition.png)

2. When the `Add new action` wizard opens select `Change field values of data` and click on _OK_. This will automatically select the `FraudData` object, as this is the only object that has been bound to a variable.

  ![Business Central Guided Rule Modify Fraud Data Wizard](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-guided-rule-modify-fraud-data-wizard.png )

3. Now set the value of the property `automated` to `true`, indicating that an automatic chargeback applies. Click on the action `Set value of FraudData [data]` and select the field `automated`. Click on the pencil icon to the right and assign a literal value to the property.

    ![Business Central Guided Rule Modify Fraud Automated](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-guided-rule-modify-fraud-automated.png)

4. Select `true` as the value for the automated property (this is the default value for booleans, so the property is probably already set to `true`). Note that since the type of data is `boolean`, you can only choose between `true` and `false`.

    ![Business Central Guided Rule Modify Fraud Automated True](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-guided-rule-modify-fraud-automated-true.png)

5. To validate that everything is correct, click on the _Validate_ button on the top navigation bar and you should see a green "Item successfully validated!" message.

    ![Business Central Guided Rule Validate](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-guided-rule-validate.png)

6.  Finally, click on _Save_ to save the rule.

You have created your first Business Rule using the Guided editor

# Lab 3 Decision Services

In this section you will learn:

1. How to use the rules that you authored in the previous step;
2. How can you expose your decisions as a service;
3. How to use your automated decisions and rules.

In previous labs we have defined the Business Object Model and the rules and decisions that operate on the model.

Let's start by having an overview of assets versioning with Business Central.

## Assets Versioning

As you can notice on the diagram below, Red Hat Process Automation Manager is a modular platform to develop and run decisions and processes. Until now we have mostly used **Business Central** workbench to develop rules and business models. All the assets that you create or alter using Business Central are versioned in a repository (a Git-based version control system).

![RHPAM 7 Architecture](https://github.com/relessawy/dm_lab_instructions/blob/master/images/rhpam-7-architecture.png)

Business Central automatically versions our code within git-based repository, and it also provides a way for us to check the changes and rollback to previous versions necessary.

1. Go to your project library view and select the automated-chargeback rule. Once the editor opens click on the button Latest Version. (**NOTE:** If you re-imported the project then there is probably only 1 version listed).

  ![Business Central Chargeback Versions](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-chargeback-versions.png)

2. There is also more metadata about your assets stored in the repository, like the name of the user that created the asset, the time and data when it was last modified, etc.

  ![Business Central Chargeback Versions Detail](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-chargeback-versions-detail.png)
  
3. Click on any of the versions in the dropdown box and you will see the version of the asset tagged.

  ![Business Central Chargeback Version](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-chargeback-version.png)

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

![Business Central Process Server Server Configurations](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-server-configuration.png)

2. Return to the Home screen and select _Design_. Select `MySpace`, next, select your Credit Card Dispute project (`ccd-project`). The Library view should open with a list of all your assets. These assets will be compiled and packaged inside a _KJAR_, a _Deployment Unit_.

3. Click on the _Deploy_ button in the top right corner.

  ![Business Central Deploy](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-deploy.png )

You will see that the project is first built, meaning the assets are compiled and packaged, and then deployed to a Execution Server container. Go back to the Home screen and elect Deploy. You will now see a container running with your newly created decisions.

## Execution

Let's check if the service you deployed is available.

1. Go to the Main Menu and select `Deploy`

2. Click on the URL of the container, and a new tab should open:

  ![Business Central Execution Services Detail](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-execution-services-detail.png)

3. You may also be prompted for credentials. Use the same credentials you used to log into the Business Central console.
  - user: `pamAdmin`
  - password: `redhatpam1!`

![Business Central Execution Services Info](https://github.com/relessawy/dm_lab_instructions/blob/master/images/business-central-execution-services-info.png)

4. Notice the Process Service responds with details about the `Kie Container` where your `Deployment Unit` is running.

Congratulations! Now that you have deployed your first business application within the engine, let's learn how about how to automate tests of the rules you created in the Credit Card Dispute project.

# Lab 4 Testing your Decision Service

Now that you've created your Decision Service, and have deployed it on the Red Hat Process Automation Manager execution server, let's learn how we can test it.

In this section you will:
1. Learn how to create test scenarios to validate rules implementation;
2. Learn how to execute test scenarios;
3. Test the business application you deployed, by using an external app.

# Introduction to test scenarios

There are various ways in which you can test a Decision Service. It is a good practice to write test-cases for individual rules, groups of rules, and your entire service.

These test-cases can be automatically executed when the code and rules of your service are compiled and packaged. This provides the guarantee that your service is tested properly before it's deployed into a production environment. It ensures that the logic that is executed and the decision that are made are correct and according to specification. Red Hat Process Automation Manager provides full support for these kind of testing scenarios.

Red Hat Process Automation Manager contains a sophisticated _Test Scenario_ feature, in which you can build various test scenarios for your rules.

## Test Scenario

1. Go back to the project's `Asset Library` view. Click on the _Add Asset_ blue button. Select _Decision_ in the asset filter in the upper left of the screen, to filter out the decision assets.

2. Select the `Test Scenario` tile. Give the scenario the name `risk-evaluation-tests` and set the package to `com.myspace.ccd_project`. Set the _Source Type_ to `Rule`.

    ![Test Scenario Create](https://github.com/relessawy/dm_lab_instructions/blob/master/images/test-scenario-create.png)

3. The scenario testing tool uses the concept in which _Given_ a specific set of input data, we _Expect_ a certain result. To implement our test scenario we therefore have to specify the input data of our rules (_Given_) and the results we expect our rules to produce for the given input data.

4. In the editor, click on the _Data Objects_ tab. If everything is correct, there should be 3 data types listed: `AdditionalInformation`, `CreditCardHolder` and `FraudData`.

5. Go back to the _Model_ tab. To test our rules, we need to provide the input data and the expected output. Our decision table operates on 2 datatypes, `CreditCardHolder` and `FraudData`. So let's start by creating a column for our `CreditCardHolder` in the _Given_ part of the scenario testing table. Click on the _INSTANCE 1_ cell in the table. On the right-hand-side of the editor, in the _Test Tools_ panel, expand the _Data Object_ `CreditCardHolder`, select the `status` field and click on the _Add_ button.

    ![Test Scenario Add Given CCH Status](https://github.com/relessawy/dm_lab_instructions/blob/master/images/test-scenario-add-given-cch-status.png)

    The column in the _Given_ section will be configured to represent that `status` field of the `CreditCardHolder` object.

6. To add an additional column in the _Given_ section of the table, right-click on the _Given_ column and select `Insert column right`.

    ![Test Scenario Given Insert Column Right](https://github.com/relessawy/dm_lab_instructions/blob/master/images/test-scenario-given-insert-column-right.png)

7. Click on either the cell with the word `INSTANCE` or `PROPERTY`, and in the _Test Tools_ panel, expand _Data Object_ `FraudData`, and select the field `totalFraudAmount`. Click on the _Add_ button.

8. Now that we have configured the 2 columns that define our input data, we can now configure the column in which we can set our expected result. Click on either the `INSTANCE` or `PROPERTY` cell in the `EXPECT` cell. In the _Test Tools_ panel on the right, expand the `FraudData` object, select the `disputeRiskRating` field, and click _Add_.

    ![Test Scenario Table Configured](https://github.com/relessawy/dm_lab_instructions/blob/master/images/test-scenario-table-configured.png)

9. With the table configured, we can now start adding our test-cases. Add a row with the following values:
    - Given:
        - CreditCardHolder.status: `Standard`
        - FraudData.totalFraudAmount: `42`
    - Expect:
        - FraudData.disputeRiskRating: `0`

    ![Test Scenario First Test](https://github.com/relessawy/dm_lab_instructions/blob/master/images/test-scenario-first-test.png)

10. Run the test by click on the _Play_ button in the top menu (next to the _Close_ button). If everything is correct, the test will run and the result will be shown.

11. Add some additional tests to complete your scenario tesing, for example:
    - Given:
        - CreditCardHolder.status: `Gold`
        - FraudData.totalFraudAmount: `863`
    - Expect:
        - FraudData.disputeRiskRating: `1`

    ![Test Scenario Two Test](https://github.com/relessawy/dm_lab_instructions/blob/master/images/test-scenario-two-tests.png)

Feel free to try incorrect values to the expected column to check the behavior of the tooling.

## Test via Web Application

In the previous exercise, we used the _Test Scenario_ tooling to test the rules in our Credit Card Dispute project. Now we will test the deployed service via the REST API it exposes.

In order to do that, let's use a simple web-application that we've provided for you. The application allows you to enter the data of the credit-card holder, and the data of the line item. The data is submitted to the Decision Server, which will calculate the risk of the transaction and determine whether the data can be automatically processed.

![ReactJS App](https://github.com/relessawy/dm_lab_instructions/blob/master/images/reactjs-app.png)

1. To access the application, go back to your OpenShift environment, and click on the `react-web-app` route to open it in a new tab:

![ReactJS App Route](https://github.com/relessawy/dm_lab_instructions/blob/master/images/openshift-react-app-route.png)

2. Once you've opened the react web app, enter the following details:

  - **Name:** Jim
  - **Age:** 52
  - **Status:** Gold
  - **Description:** Delta Airlines
  - **Amount:** 1000
  

3. Next, click on the _Submit_ button. The application will send a RESTful request to the Decision Server. If everything is working correctly, the Decision Server will send a result that will be displayed in the application:

![ReactJS App Request Response](https://github.com/relessawy/dm_lab_instructions/blob/master/images/reactjs-app-request-response.png)

We can see that the `riskRating` has been set to **1** and the transaction is eligible for automated processing. Feel free to test your Decision Service with different values to see if all the use-cases you've implemented in your rules are covered.

We have now successfully tested our decision service. In the next lab we will take a look at authoring decisons using the DMN standard.


# Lab 5 Decision Model & Notation

In this lab we will create a decision that determines the number of vacation days assigned to an employee using Decision Model & Notation (DMN). The number of vacation days depends on age and years of service

* Every employee receives at least 22 days.
* Additional days are provided according to the following criteria:
1. Only employees younger than 18 or at least 60 years, or employees with at least 30 years of service will receive 5 extra days
2. Employees with at least 30 years of service and also employees of age 60 or more, receive 3 extra days, on top of possible additional days already given
3. If an employee has at least 15 but less than 30 years of service, 2 extra days are given. These 2 days are also provided for employees of age 45 or more. These 2 extra days can not be combined with the 5 extra days.

## Create a Decision Project
To define and deploy a DMN decision model, we first need to create a new project in which we can store the model. To create a new project 

- Click on the **home button -> Design -> MySpace**
- Click on the blue **Add project** button at the top right of the page
- Give the project the name `vacation-days-decisions`, and the description `Vacation Days Decisions` and click the **Add Button**.

![Create Project](https://github.com/relessawy/dm_lab_instructions/blob/master/images/add-project-vacation-days-decisions.png)


With the project created, we can now create our DMN model. Click on the blue **Add Asset** button.
In the **Add Asset** page, select **Decision** in the dropdown filter selector.

![New Asset](https://github.com/relessawy/dm_lab_instructions/blob/master/images/new-asset-decisions-filter.png)

Click on the **DMN** tile to create a new DMN model. Give it the name `vacation-days`. This will create the asset and open the DMN editor.

![Vacation Days](https://github.com/relessawy/dm_lab_instructions/blob/master/images/add-dmn-vacation-days.png)


## DMN Editor

The DMN Editor consists of a number of components:

* **Decision Navigator**: shows the nodes used in the Decision Requirements Diagram (DRD, the diagram), and the decisions behind the nodes. Allows for quick navigation through the model.
* **Decision Requirements Diagram Editor**: the canvas in which the model can be created.
* **Palette**: Contains all the DMN constructs that can be used in a DRD, e.g. Input Node, Decision Node, etc.
* **Expression Editor**: Editor in which DMN boxed expressions, like decision tables and literal expressions, can be created.
* **Property Panel**: provides access to the properties of the model (name, namespace, etc), nodes, etc.
* **Data Types**: allows the user to define (complex) datatypes.

![DRD](https://github.com/relessawy/dm_lab_instructions/blob/master/images/dmn-editor-components.jpg)

![Boxed Expressions](https://github.com/relessawy/dm_lab_instructions/blob/master/images/dmn-editor-decision-table.jpg)

![Data Types](https://github.com/relessawy/dm_lab_instructions/blob/master/images/dmn-editor-datatypes.jpg)


## Solution

Follow this step-by-step guide which will guide you through the implementation.

**Input Nodes**

The problem statement describes a number of different inputs to our decision:

* **Age** of the employee
* **Years of Service** of the employee

Therefore, we create 2 input nodes, one for each input:

Add an **Input** node to the diagram by clicking on the **Input** node icon and placing it in the DRD.

![Input](https://github.com/relessawy/dm_lab_instructions/blob/master/images/add-drd-input-node.png)

Double-click on the node to set the name. We will name this node `Age`.
With the `Age` node selected, open the property panel. Set the **Output data type** to `number`.

![Age_Input](https://github.com/relessawy/dm_lab_instructions/blob/master/images/Age_Input.png)

In the same way, create an **Input** node for `Years of Service`. This node should also have its **Output data type** set to `number`.

![Output Data Type](https://github.com/relessawy/dm_lab_instructions/blob/master/images/drd-input-nodes-complete.png)

Save the model.


**Constants**

The problem statement describes that every employee receives at least 22 days. So, if no other decisions apply, an employee receives 22 days. This is can be seen as a constant input value into our decision model.
In DMN we can model such constant inputs with a **Decision** node with a **Literal** boxed expression that defines the constant value:

Add a **Decision** node to the DRD

![Decision Node](https://github.com/relessawy/dm_lab_instructions/blob/master/images/add-drd-decision-node.png)

- Give the node the name `Base Vacation Days`.
- Click on the node to select it and open the property panel. Set the node's **Output data type** to `number`.
- Click on the node and click on the **Edit** icon to open the expression editor.

![Edit Decision Node](https://github.com/relessawy/dm_lab_instructions/blob/master/images/drd-decision-node-edit.png)

In the expression editor, click on the box that says **Select expression** and select **Literal expression**.

![Select Expression](https://github.com/relessawy/dm_lab_instructions/blob/master/images/select-expression.png)


Simply set the **Literal Expression** to `22`, the number of base vacation days defined in the problem statement.

![Select Expression](https://github.com/relessawy/dm_lab_instructions/blob/master/images/base-vacation-days-literal-expression.png)


Save the model.

**Decisions**

The problem statement defines 3 decisions which can cause extra days to be given to employees based on various criteria. Let's simply call these decision:

* Extra days case 1
* Extra days case 2
* Extra days case 3

Although these decisions could be implemented in a single decision node, we've decided, in order to improve maintainability of the solution, to define these decisions in 3 separate decision nodes.

In your DRD, create 3 decision nodes with these given names. Set their **Output data types** to `number`.
We need to attach both input nodes, **Age** and **Years of Service** to all 3 decision nodes. We can do this by clicking on an Input node, clicking on its arrow icon, and attaching the arrow to the Decision node.

![three_decision_nodes](https://github.com/relessawy/dm_lab_instructions/blob/master/images/add-drd-three-decision-nodes.png)


Select the **Extra days case 1** node and open its expression editor by clicking on the **Edit** button.
Select the expression **Decision Table** to create a boxed expression implemented as a decision table.
The first case defines 2 decisions which can be modelled with 2 rows in our decision table as such:
employees younger than 18 or at least 60 years will receive 5 extra days, or ...
employees with at least 30 years of service will receive 5 extra days

![case1](https://github.com/relessawy/dm_lab_instructions/blob/master/images/decision-table-case-1.png)


Note that the **hit-policy** of the decision table is by default set to `U`, which means `Unique`. This implies that only one rule is expected to fire for a given input. In this case however, we would like to set it to `Collect Max`, as, for a given input, multiple decisions might match, but we would like to collect the output from the rule with the highest number of additional vacation days. To do this, click on the `U` in the upper-left corner of the decision table. Now, set the **Hit Policy** to `Collect` and the **Builtin Aggregator** to `MAX`.

![Decision Table Hit Policy](https://github.com/relessawy/dm_lab_instructions/blob/master/images/decision-table-hit-policy.png)
 
Finally, we need to set the default result of the decision. This is the result that will be returned when none of the rules match the given input. This is done as follows:
- Select the output/result column of the decision table. In this case this is the column `Extra days case 1`
- Open the properties panel on the right-side of the editor.
- Expand the **Default output** section.
- Set the `Default output property` to `0`.

![Decision Table Default Output](https://github.com/relessawy/dm_lab_instructions/blob/master/images/decision-table-default-output.png)

Save the model

The other two decisions can be implemented in the same way. Simply implement the following two decision tables:

![case2](https://github.com/relessawy/dm_lab_instructions/blob/master/images/decision-table-case-2.png)

![case3](https://github.com/relessawy/dm_lab_instructions/blob/master/images/decision-table-case-3.png)

**Total Vacation Days**

The total vacation days needs to be determined from the base vacation days and the decisions taken by our 3 decision nodes. As such, we need to create a new Decision node, which takes the output of our 4 Decision nodes (3 decision tables and a literal expression) as input and determines the final output. To do this, we need to:

- Create a new Decision node in the model. Give the node the name `Total Vacation Days` and set its **Output data type** to `number`.
- Connect the 4 existing Decision nodes to the node. This defines that the output of these nodes will be the input of the next node.

![DRD Complete](https://github.com/relessawy/dm_lab_instructions/blob/master/images/drd-complete.png)


Click on the `Total Vacation Days` node and click on **Edit** to open the expression editor. Configure the expression as a literal exprssion.
- We need to configure the following logic:
- Everyone gets the Base Vacation Days.
- If both case 1 and case 3 add extra days, only the extra days of one of this decision is added. So, in that case we take the maximum.
- If case 2 adds extra days, add them to the total.
- The above logic  can be implemented with the following FEEL expression:

![Total Vacation Days Expression](https://github.com/relessawy/dm_lab_instructions/blob/master/images/total-vacation-days-expression.png)


Save the completed model.


**Deploying the Decision Service**

With our decision model completed, we can now package our DMN model in a Deployment Unit (KJAR) and deploy it on the Execution Server. To do this:

- In the bread-crumb navigation in the upper-left corner, click on `vacation-days-decisions` to go back to the project's Library View.
- Click on the **Deploy** button in the upper-right corner of the screen. This will package our DMN mode in a Deployment Unit (KJAR) and deploy it onto the Execution Server (KIE-Server).
- Go to the **Execution Servers** perspective by clicking on "Menu -> Deploy -> Execution Servers". You will see the **Deployment Unit** deployed on the Execution Server.

## Test DMN Solution

Now you can test your DMN model similar to what we did in lab 4
