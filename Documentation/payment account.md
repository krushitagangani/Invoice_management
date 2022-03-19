Apart from this, the system should allow the client to set up their payment account and that 
would enable the system to auto process the invoice payment. E.g. if client
sets the auto pay mode in his payment account (profile) for a specific sender (invoice generator user), 
every time, the invoice is getting created by that user, the system goes ahead and charges the credit card registered with the system


- invoice - create -> sent -> client - confirm
    - invoice created
    - invoice sent
    - invoice confirmed: {invoice_id, client_id, payee_id)
    - invoice_paid
    - invoice_failed


- listen to invoice_confirmed event:
    - listener - which consumes the events i.e. invoice_confirmed
    - listener will find out payment account
        - it uses the payment_method to perform "transaction"
        - updates transaction, invoice
    - listener will inform someone that it has finished it's job - invoice_paid or invoice_failed
        

- listen to invoice_paid or invoice_failed:
    - inform client by sending email and user


transaction/payment
    - id
    - type
    - invoice_id
    - status: initiated, processsed, success, failed, rejected


payment account
 - id
 - owner_id (client_id)
 - payment_method_id
 - balance - 
 - payee_id - user_id

payment_method:
    - id
    - type: credit_card, bank_account, etc
    - meta - credit_card info


Payment scheduling
==================
The system should also allow scheduling the payments. E.g. The client can set/select a schedule and the user with a fixed invoice payment amount
(E.g. every month charge $4600 for software consulting work).


- invoice - auto generation (runs every day)
   - find payment_schedule matching with today
   - auto generated invoices - invoice_created
   - initiate payment - raise event - invoice_confirmed


- payment_schedule
 - id
 - client_id
 - user_id
 - frequency
 - particulars
    - description
    - qty
    - amount