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
    - we need to receive the emails every time
    - the received email should be in fixed format (as above) with name, contact, question, email can be parsed from the received email
    - the system would need to parse the received email(store the data of the fixed templated email, data like: name, contact, question, email) and would need to store these details somewhere
    - the system should able to auto send the question related response to the sender(take the email from the records)
    
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
    - once tag assigned, the system would need to respond to the sender by email.(from the record, system have)
    - every tag will have the specific template to respond for the specific sender and question related with tag, so system would need to store templates somewhere having tags associated with it(the answers associated with it)

-----------------------------------------------------------------------------------------

- Problem: The system should also create a ticket in the system automatically and assigned the ticket to the person available in the team. Every person available in the team belongs to a specific team (e.g. Software development, sales, etc), based on the assigned tags, the ticket should be created and then assigned to the next available person in there.

Solution:
    - the ticket should be created with the relative team and person, so...
        - we need to create the person & team first
        - assign the team to the each person
        - team should be associated with the tag
    - as soon as tag assigned, system would need to create and store the ticket details automatically(related with the tags(tag is associated with the question)) with the team & person details
    - during the ticket creation, we would need the availablility status of the person from the team, and would need to check the availablility & then need to assign to him/her
    - if no one is free from the team, then system should record the queue of newly created-unsolved ticket, and as soon as the person gets available, the FIFS(First In, First Serve) ahould apply for the pending ticket should assign to that person

-----------------------------------------------------------------------------------------

Detailed Flow:
==============
1. registration for the admin user (which consumes the business email)
2. login for the admin
3. receive the emails every time(BG process)
4. check that email should be in fixed format (so that we can parse it and takeout the required details)
5. on the background process, system will take the sender details and store it to the data base
6. after the sender info insertion, the system will relate the sender record(question) with the tag and will reply with associated template
    - for this we will need to store tags
    - Question: what's the scenario when realeted tag is not available in the stored tags?
    - we need to associate tag with the formated template(each tag will contain each template)
    - and send a mail to the sender with the related template
7. as soon as tag assigned, system will need to create ticket and assign to the available person from the specific team associated with the assigned tag(to the question)
8. the team person should be available to assign the task, so system will need to check wheather person is available ot not, if available then it will assign the task, else it will make a queue and apply FIFO for the task assignment process.


Note/Request: will go to the features listing & schema design after ur suggestion/approval