> Events
- listner: invoice_confirmed
- payload: { invoice_id } // invoice_confirmed can be retrive from invoice record
- call back = ({payload}) => {
    1. find payment account by client_id as owner_id & user_id as payee_id from payment_account table
        - Validation: invoice_id row should match with the client_id & user_id in payment_account
        - Error(User Defined): payment account not found/ invalid client_id | user_id | invoice_id,
        Invoive shoulb ne in invoices table
        - Error(System Defined): if there is no record of invoice, then throw the record not found error 
    2. get the data by invoice_id and update the status="confirmed" into the invoices table by invoice_id
        - Valiadtion: the current invoice status(before updation) should be "sent"
        - Error(User Defined): the invoice is already confirmed/paid/rejected
    3. two scenarios are here,
        - if payment account will found:
            - desc: (then it indicates that there is auto pay mode/scheduled invoice/payment account is exist with the perticular user_id)
            - to do: 
                - A. get the invoice data from invoices table by invoice_id
                - B. add the balance into net amount from the invoice.
                    - Validation: the net amount from invoice & balance from payment_account should be exist in table
                - C. get the payment method by payment_method_id from payment_method table
                    - Validation: the payment_method data should be exist
                    - Error(User Defined): if payment method not found, then throw the record not found error
                - D. insert the data into "transaction" table with status="initiated" with invoice_id
                    - Validation: the record should be a new record, means the record with associated invoice_id should not be existed(before insertion)
                    - Error(User Defined): if record is already exist, then throw the record is already exist in the system
                - E. call the "payment_processing" function for the payment gatway and pass the data as an argument
                - Error(System Defined): if there is any error in emitting event or any causes for event is not being called, then throw an 404 error
                    
        - if account will not found:
            - desc: auto pay mode is off
            - to do: return; and stop the event process
            - Error(User Defined): payment account not found invalid client_id | user_id | invoice_id

    4. handle the error by event.on('error', callback) and emit event.emit(error), in which we can display an error message & stop the event process
}

- function: payment_processing
- arguments: { amount, ...other }
- schema: ({arguments}) => {
    - update the data into "transaction" table with status="processed" by invoice_id
        - Validation: get the data from transaction by invoice_id and the current status(before updation) should be "initiated"
        - Error(User Defined): if status is initiated already then throw an error
    - send the argument and interact with payment gatway.
        - Validation: all the neccessary arguments are resuired for the payment gatway.
    - if transaction is succed then call the invoice_paid function
    - if failed then call the event invoice_failed
        - Note: format the message and throw an error for failed scenarios, e.g. the bank account not found, or payment rejected by payment gatway etc.
    - handle the error by event.on('error', callback) and emit event.emit(error), in which we can display an error message & stop the event process
    - Roll Back: true
}

- function: invoice_paid
- arguments: { invoice_id }
- schema: ({arguments}) => {
    1. find the transaction from transaction table by invoice_id and update the status="success"
        - Validation: find the transaction data and the current status(before updation) should be "processsed"
        - Error(User Defined): if status is processsed already then throw an error
    2. update the status="paid" from the invoice table by invoice_id
        - Validation: find the invoice data and the current status(before updation) should be "confirmed"
         - Error(User Defined): if status is paid already then throw an error 
    3. send the email to the user & client that payment is done
        - Validation: find the transaction data and the current status(before updation) should be "success" and the invoice status should be "paid"
    4. handle the error by event.on('error', callback) and emit event.emit(error), in which we can display an error message & stop the event process
}

- function: invoice_failed
- arguments: { invoice_id }
- schema: ({arguments}) => {
    1. find the transaction from transaction table by invoice_id and update the status="failed"
        - Validation: find the transaction data and the current status(before updation) should be "processsed"
        - Error(User Defined): if prev status is not "processsed" then throw an error
    2. update the status="failed" from the invoices table by invoice_id
        - Validation: find the invoice data and the current status(before updation) should be "confirmed"
        - Error(User Defined): if previous status is not "confirmed" then throw an error
    3. send the email to the user & client that payment is failed
        - Validation: find the transaction data and the current status(before updation) should be "failed" and the invoice status should be "failed"
    4. handle the error by event.on('error', callback) and emit event.emit(error), in which we can display an error message & stop the event process
}

- listner: scheduled_invoices
- payload: null
- callback: () => {
    1. find the all records from payment_schedule by matching today with frequency field
    2. emit event "create_scheduled_invoice" (iterate the array of invoice_id by using loop)
        - Reason: all scheduled invoice processes should be independent with each other.
        - Validation: the array of invoice_id shuld not an empty array
}


Scheduling
==========
- listner: create_scheduled_invoice
- payload: schedule_id
- callback: ({payload}) => {
    2. get the data from payment_schedule by schedule_id.
        - if record found, then create the invoice with the data like, client_id, user_id, amount, desc from the schedule record
            - Validation: the required data are should be there, the api response should return the invoice_id
            // how should we check that invoice is already exist?
            - Error(User Defined): data are missing
            - send an email to the client that the invoice is generated and update the status="sent" to the invoice from invoices table
                - Validation: the previous status of invoice should be "Generated" and invoice_id, client_id, user_id should be there.
                - Error(User Defined): missing of invoice_id, client_id, user_id / prev status is not "Generated"
                - Error(System defined): Error in sending a mail
            - pass the { invoice_id, client_id, user_id } to the "invoice_confirmed" event.
                - Validation: the previous status of invoice should be "Generated".
                - Error: the prev status is not "generated", 
            - handle the error by event.on('error', callback) and emit event.emit(error), in which we can display an error message & stop the event process
        - if record not found then return; and stop the event process
}

Error Handeling
===============
- if there is any error in event, emit "error" with throw error call back
- or we can use errorMonitor too
- we can create a class wich emits an event, and error events too
- User Defined Errors:
  ====================

- System Defined Errors:
  =====================


When to roll back?
==================
- invoice_confirmed -> step 3>B failure.

Notes
=====
- process.nextTick() can be use to call the event inside the event


References:
===========
https://www.digitalocean.com/community/tutorials/using-event-emitters-in-node-js
https://www.digitalocean.com/community/tutorials/nodejs-event-driven-programming
https://kentcdodds.com/blog/implementing-a-simple-state-machine-library-in-javascript

Review Notes:
=============
- missed validation, state transition
- roll back if any error occurs
- transaction in database - search for it
- structure blueprint


Next Steps:
===========
Error management & Recovery



confirm
get the invoice by invoice_id
get the payment account
update the invoice
insert the transation
update status in transation
update status in invoice

