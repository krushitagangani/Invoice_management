We would like to build an invoice management system with payment automation. The user should be able to generate invoices and sent them to the respective clients.

The user can register with the system and invite their respective clients.

The client can either pay the invoice using a link received in the email or by simply login into the system, going to pending invoices, click on the respective invoice, and paying it.

Apart from this, the system should allow the client to set up their payment account and that would enable the system to auto process the invoice payment. E.g. if client
sets the auto pay mode in his payment account (profile) for a specific sender (invoice generator user), every time, the invoice is getting created by that user, the system goes ahead and charges the credit card registered with the system.

The system should also allow scheduling the payments. E.g. The client can set/select a schedule and the user with a fixed invoice payment amount
(E.g. every month charge $4600 for software consulting work).

1 user creates the invoice and then share it with client. He can indicate that the invoice is recurring 

2 client can then choose existing payment profile or create one and then associates it with invoice he received 

3 based on the schedule the system can then auto deduct everything

Invoice Management System
==========================

Features :
> User Management:
    - Authentication:
        - sign up
        - login
        - forgot password
        - reset password
    - invitation to the clients for registration
    - Dashboard:
    - Invoices:
        - Create New Invoice
        - List:
            - Invoice:  Request to pay (send the payment link to the client) 
    - BG Process : listed payment task for auto pay mode

> Client Management:
    - Authentication:
        - sign up
        - login
        - forgot password
        - reset password
    - Dashboard:
    - Invoices:
        - List:
            - Invoice: Pay the invoice mannualy by button click
        
    - Payment:
        - switch for auto pay mode
        - select user
        - select number of days
        - select fixed amount
        - Add/Update Credit card details

> Payment :
    - Auto paymode for specific sender
        - save the credit card details (for auto scheduled payments)
    - Payment Options:
        - pay by link (from email)
        - pay manually
        - schedule the pay (if mode is on, then automatically it will be paid from the saved credit card)

> Invoice Management:
    - create invoice
    - list by invoice status, pay auto mode

Resourses:
> https://braindose.blog/2022/01/05/modernize-payment-platform-event-driven-architecture/
    

Notes:
> AWS provdes the support for it


Questions:
> What is OpenAPI?
