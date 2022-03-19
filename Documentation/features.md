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
        - switch for schuleing mode
        - select user
        - select number of days
        - select fixed amount
        - Add/Update/delete/view Credit card details
        - BG Process : Schedule Payment based on number of days
            - system will generate invoice, and will pay for it automatically
            - invoice will generate in certain period of time
            - client will define the period of time, whom to pay and how much to pay
            - client will also define from which to pay
        - BG Process : listed payment task for auto pay mode 
            - system will pay the invoice as soon as invoice gets confirmed by the client
            - for the auto payment functionality, system will take a certain amount of charge from the client
            - for the payment, client has to define that from which he has to pay and whom to pay
                - auto_pay
                - payment_account

> Payment :
    - Auto paymode for specific sender
        - save the credit card details (for auto scheduled payments)
    - Payment Options:
        - pay by link (from email)
        - pay manually
        - schedule the pay (payments on the given number of days)
        - Auto Pay mode on (if mode is on, then automatically it will be paid from the saved credit card)

> Invoice Management:
    - create invoice
    - list by invoice status, pay auto mode

Review Notes:
=============
> do not make ur features based on the UI (problem mate solution - Not UI, u will have to make a solution for UI from the flow)
> Schedule - Pre-confrim > need to check/update the flow