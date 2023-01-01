# Foundation

## Fundamentals

### Preview

#### Process Preview

* Identify the main processes of CSI
* Describe the Implementation Accelerator (IA)
* Identify the end-to-end business processes for CSI
* Identify the primary types of data used in CSI

#### Lead to Order 线索到订单

* Explain how to perform **key sales management tasks** in the Lead to Order business process

* Identify the components of the Lead to Order business process
* List the steps to create a new marketing or sales campaign
* Explain the flow between new lead, new prospect, new customer, and new opportunity

#### Quote to Cash 报价到销售

* Explain how to complete the **main sales order functions** of the Quote to Cash business process
* Identify the components of the Quote to Cash business process
* Identify key elements of setting up a customer
* Describe the flow from estimate to sales order

#### Design to Release 设计到生产发布

* Explain the **primary functions** of the Design to Release process
* Identify the components of the Design to Release business process
* Explain how to define the components of the bill of materials (BOM)
* List the steps to create a new bill of materials

#### Procure to Pay 采购到付款

* Explain the **key vendor management tasks** in the Procure to Pay business process
* Identify the components of the Procure to Pay business process
* Explain how to create a purchase order (PO) for items not used in the manufacturing process
* Describe user limits for requisition approvals
* Explain how to complete a PO Order from the work bench
* Describe the impact of a shrink factor on material quantities
* Explain how to receive a PO order
* Identify resources for reviewing the system impact of Procure to Pay processes

#### Plan to Inventory 库存计划

* Explain **how to use Advanced Planning and Scheduling (APS)** in the Plan to Inventory business process
* Identify the components of the Plan to Inventory process
* Identify system planning engine options
* Describe the benefits of using the material planner workbench
* Explain how to create the workbench from planning output

#### Demand to Build 需求到制造

* Identify the steps in the Demand to Build process
* Identify the components of the Demand to Build business process
* Identify the two different Plan to Schedule processes
* Describe the flow of completing jobs
* Describe customer order shipping
* Identify resources to review the system impacts of Demand to Build business processes

#### Financial Plan to Report 财政计划到报告

* Describe the primary financial management functions in the Financial Plan to Report business process
* Identify the components of the Financial Plan to Report business process
* Describe the flow of creating and processing customer invoices 发票 and payments
* Describe the flow of creating, processing and paying vendor POs
* Describe how to enter and post journal entries
* Identify resources to review the system impacts of the Financial Plan to Report business processes



### Process Preview 

#### Introduction to CSI Implementation Accelerator

> A set of preconfigured yet flexible solutions for Infor CloudSuite apps
>
> IAs deliver industry-specific best practice content, including pre-defined menus, processes and structures
>
> IAs are optional in on-premise installations
>
> IAs are included as part of an Infor Saas subscription

优点:

* faster time to value
* lower cost
* reduced risk



#### End-to-End Business Processes

* Quote to Cash
  * follows the sales and order functions, from sending a customer quote to obtaining an order, shipping and invoicing the order, and receiving payment
  * ![image-20211031101317412](C:\Users\bzhan\AppData\Roaming\Typora\typora-user-images\image-20211031101317412.png)
* Lead to Order
  * supports the customer relationship management (CRM) function
* Procure to Pay
  * where items used in manufacturing and assembly are purchased from vendors
  * ![image-20211031101828398](C:\Users\bzhan\AppData\Roaming\Typora\typora-user-images\image-20211031101828398.png)
* Plan to Inventory
  * uses the APS module to identify the materials required to fulfill orders
  * ![image-20211031102507895](C:\Users\bzhan\AppData\Roaming\Typora\typora-user-images\image-20211031102507895.png)
* Design to Release
  * supports product design and the manufacturing bill of material for the product
  * ![image-20211031101510503](C:\Users\bzhan\AppData\Roaming\Typora\typora-user-images\image-20211031101510503.png)
* Demand to Build
  * focuses on the assembly or manufacturing of materials into a finished good to meet customer orders (demand)
  * Manufacturing jobs are released to the shop floor, resources are scheduled, and manufacturing process are completed
  * The Demand to Build business process includes the steps to identify and purchase or produce product inputs, and then to manufacture or assemble the finished goods and ship them to customers
  * ![image-20211031102836782](C:\Users\bzhan\AppData\Roaming\Typora\typora-user-images\image-20211031102836782.png)
* Inspect to Correct
* Service to Cash
* Service Contract to Cash
* Project to Profit
* Hire to Retire
* Financial Plan to Report
  * encompasses 包含 the organization's finance and accounting functions
  * ![image-20211031103209119](C:\Users\bzhan\AppData\Roaming\Typora\typora-user-images\image-20211031103209119.png)



#### Scenario Example

> Progressive Cycles 生产Smartwatch
>
> Task:
>
> ​	You will be assigned the task of entering all required logistical, manufacturing, accounting and financial data to create a fitness tracking Smartwatch into CSI
>
> When setting up the new Smartwatch product, you will need to consider:
>
> 	* Demand
> 	* Product Components
> 	* Logistical data (customers, vendors)
> 	* Operational data (items, work centers, resource groups, and production employees)
> 	* Bill of Materials (BOM)
> 	* Accounting data

**Demand**

described in the **Lead to Order** and **Quote to Cash** business processes

**Product Components**

manufactured by a combination of purchased parts and materials :

* Smartwatch LCD face
* Watchstrap 表带
* Watch battery

**Logistical Data**

to complete sales orders and jobs

**Operational Data**

* Items:
  * Items include all items bought, manufactured and sold. Finished good, sub-assemblies, raw materials, tooling, fixtures, and even outside services should have item records
* Resources and resource groups
  * A resource is an entity, such as crewperson 工作人员, machine, or fixture that can perform a production activity
  * A resource group is a set of similar resources used in a production operation
* Work centers
  * Work centers are used to track the costs that production activities generate
* Employees
  * Employee records include identifying information about each employee along with pay and shift information
  * This data is used to generate labor costs for production activities

**BOM**

* key components of this process include:
  * BOM
  * Job Orders
* The BOM is a comprehensive list of raw materials, components and assemblies required to build or manufacture a product
* A BOM is usually in  a hierarchical format, with the topmost level showing the end product, and the bottom level displaying individual components and materials

![image-20211031105732138](C:\Users\bzhan\AppData\Roaming\Typora\typora-user-images\image-20211031105732138.png)

Before generating a BOM or job order, two additional types of manufacturing data should be set up : 

* Current operations
  * The current operations form defines the list of resources and time required to perform a step, or operation, of the manufacturing process for an item
  * It also defines routing and costs for the item
* Current materials
  * The current materials form specifies the materials used for an operation

**Accounting Data**

* Accounting data is set up during the initial deployment of the system
* Transactional data for an organization is captured in the general ledger 总账, which is tied to other portions of the system through distribution journals and the chart of accounts
* The chart of accounts defines account numbers utilized throughout the system to record and track transactional activity
* Transactional data captured through day-to-day operations is posted to distribution journals which are in turn posted to the general ledger



### Lead to Order

> * supports the customer relationship management (CRM) function within an organization
> * a method for managing the entire sales process, including creating a sales campaign, lead generation, sales opportunity tracking, converting prospects to customers, and converting an opportunity to a customer order
> * provide a link between the sales and marketing department (front end of the business) and the operations departments (back end of the business)
> * goal: to streamline the sales and the customer service processes to generate revenue

![image-20211031100937309](C:\Users\bzhan\AppData\Roaming\Typora\typora-user-images\image-20211031100937309.png)

**Key capabilities**

CRM allows you to manage your marketing campaigns, leads, opportunities, and sales contacts to:

* Track all elements of a marketing or sales campaign, including its effectiveness
* Create leads for prospects and customers from initial interest through order placement
* Convert a prospect to a customer
* Create opportunities to track estimate orders assigned to prospects or customers
* Manage all tasks associated with an opportunity
* Maintain sales contacts including territory information, company revenue, and number of employees for each customer
* Cross-reference your sales contacts with customers and prospects
* Create and manage sales contact communications and interactions



#### Campaign to ROI

ROI: return on investment

The Lead to Order process often starts with a new marketing campaign. The marketing group can track all elements of a marketing or sales campaign, including its effectiveness. (Using **Campaign form** in CSI)

The campaign's ROI is estimated by evaluating the cost versus targeted revenue.

The effectiveness is calculated by measuring actual revenue versus estimated.

Actual revenue is captured by linking the leads and opportunities to the campaign.

**Process**

![image-20211105090840597](C:\Users\bzhan\AppData\Roaming\Typora\typora-user-images\image-20211105090840597.png)

The new campaign is created using the Campaign form, where you enter the Campaign, Description, type, status, cost, start and end dates.



##### Campaign Form

* Campaign Field
* Description Field
* Type field
* Status field



#### Lead to Opportunity

The Lead to Opportunity process includes the functions required to identify and track sales leads, and to develop those leads into sales opportunities.

Leads are those prospects or customers that fit your organization's qualifying criteria. The goal is to interact with the sales contact at the lead to evaluate requirements and establish a business relationship. The desired outcome is a sales opportunity that can be responded to by a quote or estimate.

![image-20211105092948957](C:\Users\bzhan\AppData\Roaming\Typora\typora-user-images\image-20211105092948957.png)

* A new lead is recorded on the Leads Form, which includes information about the lead status, quality, and source, along with information about any record campaign.
* A sales person is assigned to the lead while the lead is also associated with a prospect or a customer.
* Interactions between the salesperson and the sales contact at the prospect or customer are logged in the system.
* The salesperson's goal is to develop this lead into an opportunity

##### Lead Form

* Lead field
* Description field
* Source field
* Status field

Lead from can use Campaign field to link to a campaign.

##### Prospect Form

* Prospect field will be assigned automatically when saved
* Company field
* Address 1 field
* City field
* Prov/State field
* Postal/Zip field

##### Opportunity Form

* Opportunity field

* Prospect field (link to a prospect)
* Customer field (link to a customer)
* Status field
* Campaign field (link to a campaign)



#### Prospect to Customer

The prospect to Customer process allows you to convert an existing prospect to a customer.

The customer attributes are identified  based on information from the sales and accounting departments, and are recorded on the Customers form.

![image-20211105101920109](C:\Users\bzhan\AppData\Roaming\Typora\typora-user-images\image-20211105101920109.png)

The Prospect to Customer process begins when an opportunity is won, and a customer order is received from the prospect.

1. Before the prospect is converted into a customer, a credit check is conducted to verify that the prospect meets the organization's requirements.
2. Next, the prospect is converted to a customer by running the Move to Customer utility.
3. Then the resulting new customer record is updated with credit limit, payment terms and contact information. The customer order can now be entered.

On the prospect form, click the ellipsis and click the `Move to Customer`. The Move to Customer form opens. Enter `New Customer` field and `Bank Code` field.

Once the prospect has been converted to a customer, the prospect record is deleted. The new customer can be viewed on the Customer360 form.



### Quote to Cash

> The Quote to Cash business process helps organizations manage their sales activities. This business process encompasses all the activities starting from submitting quotes to customers, winning the order, shipping the items and finally invoicing and collecting payments.

![image-20211106101942640](C:\Users\bzhan\AppData\Roaming\Typora\typora-user-images\image-20211106101942640.png)

Five components:

* Quote to Sales Order
* Sales Order to Shipping
* Shipping to Invoice
* Invoice to Cash
* Return to Credit

**Key capabilities**

* Track opportunities
* Record order shipments
* Invoice and collect payments
* Manage credit returns

#### Quote to Sales Order

![image-20211106105704078](C:\Users\bzhan\AppData\Roaming\Typora\typora-user-images\image-20211106105704078.png)

An opportunity is an event with associated tasks for a prospect or customer. The process of developing an opportunity into a sales order usually includes the following steps:

* When an opportunity is identified, a new record is added on the Opportunities form. The opportunity can be linked to an existing lead.
* A prospect or customer is associated to the opportunity.
* A salesperson and a sales contact from the prospect or customer can also be assigned to the opportunity.
* Follow-up tasks can be recorded and tracked.
* An estimate is created and linked to the opportunity record.
  * Estimate lines are created for each item that is being quoted, including quantity, price, and due date.
  * The estimate provides a breakdown to the customer of the costs and the delivery times of the goods or services to be provided. Each estimate includes one or more estimate lines, which specify the item and quantity being quoted.
* A quote is generated using the Estimate Response Form Report and sent to the prospect or customer
* Once the prospect or customer is satisfied with the quote, the opportunity is awarded.
  * If the won opportunity is for a prospect, that prospect must be converted into  a customer
* The estimate order is converted into a customer order.

##### Assign task to opportunity

Open the `Opportunity Task` form from the `Tasks` button on the selected opportunity on the `Opportunity` form.  

Enter the `Task Type` and `Task` field.
