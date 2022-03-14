Schema Design:
==============
users:
    - id
    - first_name
    - last_name
    - email
    - address
    - mobile
    - password

clients:
    - id
    - first_name
    - last_name
    - email
    - address
    - mobile
    - password
    - auto_payment

invoices:
    - id
    - user_id
    - client_id
    - title
    - desc
    - summary
    - desc
    - notes
    - terms_and_conditions
    - total
    - invoice_status
    - due_date
    - discount

taxes:
    - id
    - name
    - percentage
    - invoice_id

invoice_items:
    - id
    - invoice_id
    - name
    - desc
    - price
    - discount
    - qty

payments:
    - id
    - invoice_id
    - payment_status
    - amount
    - mode (standard, scheduled, auto_pay)
    - due_date
    - paid_from

credit_cards:
    - id
    - client_id
    - card_number
    - cvv
    - card_name
    - expiration_date

scheduled_invoices:
    - id
    - invoice_id
    - client_id
    - user_id
    - period
    - fixed_amount

auto_payment_invoices:
    - id
    - invoice_id
    - charge_amount
    - credit_card_id
    
