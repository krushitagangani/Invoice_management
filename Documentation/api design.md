> User
    - Authentication
        - registration
            - url: auth/register
            - method: POST
            - body params: {}
            - query params: null
            - validations:

        - login
            - url: auth/login
            - method: POST
            - body params: {}
            - query params: null
            - validations:

        - forgot_password
            - url: auth/forgot-password
            - method: POST
            - body params: {}
            - query params: null
            - validations:

        - reset_password
            - url: auth/forgot-password
            - method: PUT
            - body params: {}
            - query params: null
            - validations:

> Client
    - registration
            - url: auth/register
            - method: POST
            - body params: {}
            - query params: null
            - validations:

        - login
            - url: auth/login
            - method: POST
            - body params: {}
            - query params: null
            - validations:

        - forgot_password
            - url: auth/forgot-password
            - method: POST
            - body params: {}
            - query params: null
            - validations:

        - reset_password
            - url: auth/forgot-password
            - method: PUT
            - body params: {}
            - query params: null
            - validations:

> Invoices:
    - Create an Invoice:
        - url: invoices/create
        - method: POST
        - body params: {}
        - query params: null
        - validations:

    > Get Invoices:
        - url: invoices/list
        - method: GET
        - body params: {}
        - query params: [status, auto_payment, scheduled_payment, pageIndex, limit, sort_by ]
        - validations:

    > Get Invoice Details:
        - url: invoices/{id}
        - method: GET
        - body params: null
        - query params: invoice_id
        - validations:

    > Update invoice Status:
        - url: invoices/{id}/update-status
        - method: PATCH
        - body params: {}
        - query params: invoice_id
        - validations:

    > Delete Invoice:
        - url: invoices/{id}/delete
        - method: Delete
        - body params: null
        - query params: invoice_id
        - validations:

> 
