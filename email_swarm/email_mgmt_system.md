# Email Management System

## Goal
Implement an agents swarm designed to process incoming emails from an Outlook account, analyze the content, check with SQLite database for any historical context, provide the user with a recommended reply. System should use SQLite database tables and python logic as a rudimentary and basic memory system for the application to keep current state and some form of recent, short-term and long-term historical snapshots.

## Features

- Connects to Microsoft Outlook via win32com.client to access and analyze emails.
- An incoming email should trigger the email analysis process and return the results.
- Processes emails from specified folders, identifying replied to versus unreplied emails, summarizing their content, etc.
- For each email, checks if it has been replied to and whether it exists in the database.
- If unreplied, generates a summary of the email and overview of any historical content from the database if any exists.
- System should also have an open items/tasks/to-do list based on incoming/sent emails.

## Email Tracking

Email messages should be tracked, stored, identified, etc., per the following:
- Has it been replied to (check in the sent items folder)
- Is the email a single message or part of a thread
- Any others you recommend needed to meet the goal 

## Database Management

- Use SQLite database to store email data, maintain a database of received emails, sent emails, etc.
- All primary email data (subject line, message body, sent/received time, sender, receiver, etc.) should be saved to the database along with any valuable metadata.
- Provide database functions to check if an email exists, insert new emails, retrieve emails.
- Use LLM API to generate summaries of email content based on 1-day summaries and 1-week summaries, and these summaries should be stored in the database.

## Current Database Schema @email_data_schema.json

The current database schema consists of multiple tables, each representing a different email folder or category. All tables share the same structure with the following columns:

1. id (INTEGER, AUTOINCREMENT)
2. email_id (TEXT)
3. subject (TEXT)
4. body (TEXT)
5. sender (TEXT)
6. receiver (TEXT)
7. sent_time (TEXT)
8. received_time (TEXT)
9. replied_to (INTEGER)
10. in_thread (INTEGER)
11. summary (TEXT)
12. classification (TEXT)
13. conversation_id (TEXT)

The tables included in the schema are:

1. AMP
2. AWC
3. BeUsa
4. Champion
5. Dynamis
6. Electro_Quip
7. Electro_Tech
8. EnQuest
9. Evolution
10. Fernando
11. HMH
12. Inbox
13. Jelec
14. Kinetic
15. Luke
16. Quadvest
17. SLB_CAM
18. sent_items

This structure allows for efficient storage and retrieval of email data across various categories or folders, while maintaining a consistent format for all email entries.

## Table Example

Here's an example of how the "Inbox" table structure would look:

| Column Name    | Data Type | Description                                     |
|----------------|-----------|-------------------------------------------------|
| id             | INTEGER   | Auto-incrementing primary key                   |
| email_id       | TEXT      | Unique identifier for the email                 |
| subject        | TEXT      | Email subject line                              |
| body           | TEXT      | Full email body content                         |
| sender         | TEXT      | Email sender's address                          |
| receiver       | TEXT      | Email recipient's address                       |
| sent_time      | TEXT      | Time the email was sent                         |
| received_time  | TEXT      | Time the email was received                     |
| replied_to     | INTEGER   | Flag indicating if the email has been replied to|
| in_thread      | INTEGER   | Flag indicating if the email is part of a thread|
| summary        | TEXT      | Generated summary of the email content          |
| classification | TEXT      | Email classification or category                |
| conversation_id| TEXT      | Identifier for the conversation thread          |

This table structure is replicated across all the other tables in the database, allowing for consistent data management across different email folders or categories.

## Sample Data: AMP Table @amp.json

The AMP table contains email data specific to the Accelerated Mobile Power (AMP) category. Here's an analysis of the sample data:

1. The table contains two email entries, both part of the same conversation thread.
2. Both emails have the same subject: "Re: [External] Reference Plant HMI Visualizat; AWC Quotation #2833395"
3. The sender for both emails is "Colleen.Turley@acceleratedpower.com"
4. The receivers are "Luke Rodrigue" and "Miguel Grillo"
5. Both emails were sent and received on "2024-07-16 11:23:26" and "2024-07-16 11:23:43" respectively
6. The emails are part of a thread (in_thread = 1)
7. The conversation_id is "59D3C20E555B744CB201E80130610947"
8. The email bodies contain a chain of previous communications, discussing:
   - A request for a progress update on the plant HMI
   - Scheduling a kickoff meeting
   - Confirmation of work beginning on the Plant HMI
   - Discussion about issuing a PO (Purchase Order) for the work
   - Initial quotation for the work

9. The 'classification', 'summary', and 'replied_to' fields are null, indicating that these aspects haven't been processed or determined yet.

This sample data demonstrates how the email management system captures and stores detailed information about email communications, including the full conversation history. It allows for tracking of project-related discussions, scheduling, and business processes like issuing purchase orders.
