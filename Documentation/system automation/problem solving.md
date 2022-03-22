Build an automation system that performs some of the daily tasks on behalf of you

Let's say you have a business email available with you.You are asking people to send out their business enquiries on this email. Everytime, we receieve the email on this email address, the system should parse the email and then auto respond to the sender with templatize (fixed) response. The email is going to have fixed format E.g.

Hi,

I am looking for some requirements below:

Name: John Deo
Contact: +1 856 545 4544
Question: Do you provide any data automation services?

The system tries to tag the inquiry based on the question text. (You don't need to worry about tagging at the moment). We can tag manually as well. Once the tags are assigned, the system should respond the sender by sending email accordingly. The response template can be decided based on the assigned tags. E.g. for above email we can assign tags - automation service, software consulting

The system should also create a ticket in the system automatically and assigned the ticket to the person available in the team. Every person available in the team belongs to a specific team (e.g. Software development, sales, etc), based on the assigned tags, the ticket should be created and then assigned to the next available person in there.

Problem Solving statements:
==========================
Core Flow:
1. system will have bussiness email
2. any outworld user can ask question on that(with fixed format)
3. then system will define the tags based on that question
4. e tag according automatic reply karshe
5. apart from that e ticket generate karshe and tag according related dept ma related person ne aapshe


- Problem: Let's say you have a business email available with you. 
Solution: 
    - need a business email
    - needs 2 roles, 
        - admin: receives the emails and sends the response to the senders
        - non-admin: requires for the task management
-----------------------------------------------------------------------------------------

- Problem: You are asking people to send out their business enquiries on this email.
Solution:
    - need something(like a page which contains list of email) which can receives & see the emails every time(every min/sec)
    - only admin (which holds the business email) can see the received emails.

-----------------------------------------------------------------------------------------

- Problem: Everytime, system will receieve the email on this email address, the system should parse the email and then auto respond to the sender with templatize (fixed) response. The email is going to have fixed format E.g.
    Hi,
    I am looking for some requirements below:
    Name: John Deo
    Contact: +1 856 545 4544
    Question: Do you provide any data automation services?

Solution:
    - parse the email and extract out name, contact, question & email.
    - let's store this information along with email's time in database
    - let the system auto assign the tag
        - described in the next point
    - let the system find the auto respond based on the assign tag

    - Another Scnario(I am suggesting): (if there is a ui, which can take the deails from user like, contact, mobile, question, email)
        - the user would need to enter the name, contact, email and the question then the system will store the details and based on that question, the system will reply on that person's email

-----------------------------------------------------------------------------------------

- Problem: The system tries to tag the inquiry based on the question text. (You don't need to worry about tagging at the moment). We can tag manually as well. 

Solution:
    - we need to create & store the tags first
    - then we can assign a perticular tag to the question (question from email), so we would need a record of the question associated with the tag
    - only system/admin user can apply tag to that question

-----------------------------------------------------------------------------------------

- Problem: Once the tags are assigned, the system should respond the sender by sending email accordingly. The response template can be decided based on the assigned tags. E.g. for above email we can assign tags (automation service, software consulting)

Solution: 
    - every tag will have the specific template to respond for the specific tag and question related with tag, so system would need to store templates somewhere having tags associated with it

-----------------------------------------------------------------------------------------

- Problem: The system should also create a ticket in the system automatically and assigned the ticket to the person available in the team. Every person available in the team belongs to a specific team (e.g. Software development, sales, etc), based on the assigned tags, the ticket should be created and then assigned to the next available person in there.

Solution:
    - the ticket should be created with the relative team and person, so...
        - we need to create the person & team first
        - assign the team to the each person
        - team should be associated with the tag
    - in order to assing the ticket to the relavent person
        - find the person in the team based on availability
            - if person is available
                - then assign the ticket to him/her
            - if person is not available
                - create a ticket without assigning anything
    - as soon as the person gets available, the FIFS(First In, First Serve) should apply for the pending ticket should assign to that person
        - as soon as the prev ticket gets resolved, then the next ticket from the queue(un-assigned ticket queue) will be assign to that person.

-----------------------------------------------------------------------------------------

Detailed Flow:
==============
2. login for the admin
3. receive the emails every time(BG process) - (incomming-received-inbound i.e. inbox, web hook)
4. check that email should be in fixed format (so that we can parse it and takeout the required details)
5. on the background process, system will take the sender details and store it to the data base
6. after the sender info insertion, the system will relate the sender record(question) with the tag and will reply with associated template
    - for this we will need to store tags
    - Question: what's the scenario when realeted tag is not available in the stored tags?
    - we need to associate tag with the formated template(each tag will contain each template)
    - and send a mail to the sender with the related template
7. as soon as tag assigned, system will need to create ticket and assign to the available person from the specific team associated with the assigned tag(to the question)
8. the team person should be available to assign the task, so system will need to check wheather person is available or not, if available then it will assign the task, else it will make a queue and apply FIFO for the task assignment process.

Features:
========
Admin:
    - Authentication (JWT)
    - can receive & see the inquiries
    - can send inquiry(will done by system)
    - can create the tag
    - can create the template related with the tag
    - can assign the tag mannually (unknown)     

Users:
    - Authentication (JWT)

inquiry:      
    - list of received inquiry
    - check validation for the received inquiriy format
    - parse email
    - store details including time in db
    - respond with the fixed template

Tags:
    - create/update/delete tags (including "unknown" tag)
    - list of tags
    - tag assignment to question (mannual/by system)
    - tag assignment to template

Team Management:
    - create, update, delete team & assign the tag
    - create, update, delete team member

Ticket Management:
    - create, update, delete ticket
    - assign the ticket (mannual/system)

Artifacts:
========
> UI:
    - React:
        - designing: react-bootstrap
        - form validations: react-hook-forms

> Data Storage: 
    - mongodb

> web server for the storage:
    - nodeJs:
        - Emails:
            > To read the emails from the email service / email protocol: SMTP
            > To Send/Respond the emails: sendGrid
            > To Parse the emails: mailparser
            > Email formats for response: Plain HTML & inline CSS 

        - Tag Auto-assign: 
            > NLP(Natural Language Processing) - Topic Detection Algorithm
        
        - BG Process Management:
            > Event driven archietecture (to handle asyc BG processes)
                - receive email
                - inquiry_create
                - tag_assignment
                    - respond to the sendr
                    - generate ticket
Schema Design:
==============
users:
    - id
    - name
    - email
    - password
    - role (ADMIN, USER)

inquiries:
    - id
    - name
    - email
    - contact
    - question
    - created_at
    - updated_at
    - tag_id

tags:
    - id
    - name

templates:
    - id
    - content
    - title
    - tags: [array of tag_id] Why array? we can have many tag id's with the same template

tickets:
    - id
    - inquiry_id
    - tag_id
    - status (CREATED, IN PROGRESS, RESOLVED, CLOSED)
    - created_at
    - updated_at
    - assignee: member_id (null/reference)
    - assigned_at
    - resolved_at

teams:
    - id
    - name
    - tags: [tag_id] why array? we can have single team with many tags
    - status (ACTIVE/DEACTIVE)

team_members:
    - id
    - user_id
    - team_id
    - status (ACTIVE/DEACTIVE)

Algorithms:
===========
> How to find the person's availibility?
    - find team by tag id
    - get all team members by team_id
    - find a records which,
        - not in the tickets entity as "assignee" field

> if task gets resolved
  
<!-- assigned_tags
    - id
    - inquiry_id
    - tag_id -->

Api Design: (will add other attributes later)
===========
> User:
    - create user
    - update user
    - delete user
    - get all users
    - getById

> Tag:
    - create tag
    - update tag
    - delete tag
    - get all tags
    - getById

> Templates
    - create template
    - getById template

> Inquiries:
    - patch
        - assign the tag (add/update tag_id)
    - get all (filters : tag_id, page, limit)
    - getById

> Tickets:
    - patch
        - update the status
    - getById
    - getAll (filters: tag, status)

> Teams:
    - create
    - getAll
    - getById
    - patch
        - relate the new tag with the team

> Team Members:
    - create
    - delete
    - find the available person(s)
        - method: GET
        - body params: null
        - query params: tag_id

Event Driven Archietecture:
===========================
Event: email_received
payload: email_details
callback: ({payload}) => {
    1. check that email format
        Validation: email should be in decided format and all 4 fields are required
        Error: the system is unable to read the email / missing contact|name|question
    2. parse the email and extract the contact, name, email, question
    3. pass all of this details to the function called "create_inquiry"
}

Function: create_inquiry
parameters: { name, email, contact, question }
defination: (parameters) => {
    1. store above parameters along with the created_at & updated_at field to the "inquiries" entity
    - Validation: { name, email, contact, question } fields are required fields
    - Error: missing field
        - the db will send newly created inquiry as a response to the system
            - Valiadtion: system should have responded with the newly added record
            - Error: inquiry_id not found, record not created    
    2. call the tag_assignment function and pass the newly created inquiry
}

Function: tag_assignment
parameters: { inquiry_id }
defination: (parameters) => {
parameters: { inquiry_id }
    1. get the record from inquiries entity by  and findout the keyword from the question field
        - Validation: inquiry_id should be valid, the record should be exist in the table
        - Error: invalid inquiry_id, unable to find the inquiry record
    2. from the above point's keyword, get the tag_id from the "tags" entity by name
            Validation: keyword should not be empty
        - if system is able to find the tag then 
            - update the inquiry record with the tag_id to the "inquiries" entity by inquiry_id
        - if system is unable to find the tag then assign with the "unknown" tag
            Validation: the "unknown" tag should be exist in the tags entity
            Error: unable to find "unknown" tag
    3. if record is updated with the tag_id, then call the respond_to_sender & create_ticket events 
}

Event: respond_to_sender
payload: { inquiry_id }
callback: ({payload}) => {
    1. get the tag_id along with the whole record by inquiry_id from the "inquiries" entity
        - Validation: inquiry_id should be valid
        - Error: inquiry_id not found, tag
        _id is not found
    2. get the templates associated with tag_id as tags from the "templates" entity
    3. if the template record is found, then
        - respond to the sender by template's content
    4. if the template record is not found then
        - respond with the "default" template
        Validation: default template should be exist in the system        
}

Event: create_ticket
payload: { inquiry_id, tag_id }
callback: ({payload}) => {
    1. check first from that ticket is already created or not
        - get record from tickets by tag_id and inquiry_id, if record found, then throw the error
        - Error: 
            - the ticket is already created with the same inquiry_id and tag_id
    2. create the ticket first
        - { inquiry_id, tag_id, status: "CREATED", created_at, updated_at }
        Validation: the ticket status should be CREATED
    3. call the assign_ticket function and pass ticket_id parameter from the newly created ticket's response object
}

Function: assign_ticket
parameters: { ticket_id }
defination: ({parameters}) => {
    1. get the ticket record from tickets by ticket_id
    2. if record found, then
        - check the status, it should be "CREATED" and the assignee field should have null value
        - Error: the ticket is already assigned, closed, resolved
    3. if no record found, then throw an error that no ticket is found
    4. find the team by tag_id and status="ACTIVE" from "teams" entity
        Validation: tag_id should be valid
        Error: tag_id not found, team is not found, team is deactivated
    5. get all team members from the "team_members" by team_id & status="ACTIVE"
        Validation: team should not be an empty
        Error: no member found, no active member found
    6. find a member_id(s) from point 2's response which are not in "tickets" entity as "assignee" field
    4. if there is more than 0 member_id(s), then 
        - use the first member_id from the array
        - update the ticket record from tickets entity by updating the fields assignee and status
            - assignee: member_id, status: "IN PROGRESS", updated_at, assigned_at
    5. if you get the empty array/no record, then return;
}


Function: resolve_ticket
parameters: { ticket_id }
defination: ({ parameters }) => {
    1. get the ticket record from tickets by ticket_id
    2. if record found, then
        - check the status, it should be "IN PROGRESS" and the assignee field should have member_id value
        - Error: the ticket is closed, resolved, ticket is not assigned before
    3. update the ticket record from tickets entity by updating the fields assignee and status
        - status: "RESOLVED", updated_at, resolved_at
    4. call the event assign_new_ticket
}

Event: assign_new_ticket
payload: null
callback: () => {
    1. get tickets from tickets entity which status="CREATED" and assignee=null
    2. if record found, then get forst record, take ticket_id and call the assign_ticket function by passing ticket_id
    3. if no record found, then
}

Function: close_ticket
parameters: { ticket_id }
defination: ({ parameters }) => {
    1. get the ticket record from tickets by ticket_id
    2. if record found, then update the ticket record from tickets entity by updating the fields assignee and status
        - status: "CLOSED", updated_at
    4. call the event assign_new_ticket
}