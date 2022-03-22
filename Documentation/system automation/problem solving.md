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

Note/Request: will go to the features listing & schema design after ur suggestion/approval

Features:
========
Admin:
    - Authentication (JWT)
    - can receive & see the emails
    - can send emails(will done by system)
    - can create the tag
    - can create the template related with the tag
    - can assign the tag mannually (unknown)     

Users:
    - Authentication (JWT)

Email:      
    - list of received emails
    - check validation for the received email format
    - parse email
    - store details including time in db
    - respond with the fixed template

Tags:
    - create/update/delete tags (including "unknown" tag)
    - list of tags
    - list of fixed templates related with each tag (including default template)
    - relate question with tag, if tag will not found then assign it to "unknown" tag)
    - find the template by tag (if template is not found then consider the default template)

Task Management:
    - create team, relate team with tag (all tags should be related with the team, except "unknown" tag)
    - create person assign it to the team
    - find the team by assigned tag
    - create the ticket along with the tag name & team
    - find the available person
    - assign the ticket to that person
    - can change the ticket status (created, processing, resolved)
    - when status gets resolved, assign FIFO created ticket to that person


Artifacts:
========
> UI:
    - React:
        - designing: react-bootstrap
        - form validations: formik hooks

> Data Storage: 
    - mongodb

> web server for the storage:
    - nodeJs:
        - Emails:
            > To read the emails from the email service / email protocol: SMTP
            > To Send/Respond the emails: sendGrid
            > To Parse the emails: mailparser
            > Question: How can we validate that received email is in decided format?
            > Email formats for response: Plain HTML & inline CSS 

        - Tag Auto-assign: 
            > NLP(Natural Language Processing) - Topic Detection Algorithm
        
        - BG Process Management:
            > Event driven archietecture (to handle asyc BG processes)

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
    - name (Default : unknown)

templates:
    - id
    - content
    - title
    - tags: [array of tag_id] Why array? we can have many tag id's with the same template

tickets:
    - id
    - inquiry_id
    - tag_id
    - status (CREATED, PROCESSING, RESOLVED)
    - created_at
    - updated_at

teams:
    - id
    - name
    - tags: [tag_id] why array? we can have single team with many tags

team_members:
    - id
    - user_id
    - team_id

assigned_tickets:
    - id
    - member_id
    - ticket_id
    - assigned_at

resolved_tickets:
    - id
    - member_id
    - ticket_id
    - resolved_at

Algorithms:
===========
> How to find the person's availibility?
    - find team by tag id
    - get all team members by team_id
    - find a records which,
        - member_id(s) from above point which are not in "assigned_tickets" entity
        
> if task gets resolved
    - update the status from "tickets"
    - delete the record from "assigned_tickets" by ticket_id
    - add the record to "resolved_tickets" entity (Purpose : to know that by whom the ticket got resolved?)

<!-- assigned_tags
    - id
    - inquiry_id
    - tag_id -->


TODO:
------
email: send grid, Mandrill 
auto assign tag: nlp(Natural Language Processing)
Features listing - done
required plugins
schema design
