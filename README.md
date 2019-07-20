# GetJiraSlaReport
[PUA][Virtual] Get Atlassian Jira Sla Report

The project is designed to receive a report on SLA compliance with support staff for processing tickets in Atlassian Jira.
On average, within the framework of this task, it is necessary to process about 300 tickets per day, for each ticket with manual labor it takes 7 minutes. Manual labor does not mean a continuous check, but only taken on a random sample, due to a limited amount of time. A robot, unlike an employee, checks all tickets in a continuous way. Thus, with manual labor, it would take 1500 minutes a day, the robot performs this work in 1 hour (of course, everything depends on the size of the ticket change history). However, according to our calculations, this robot saves 3-5 FTE.

This project considers 2 SLA indicators:
1. Reaction time (time elapsed from the moment of opening the ticket to transferring it to In-Process status) - in this example, this is 180 working minutes or 3 working hours;
2. Ticket closure time (ticket lifetime, that is, the time period for which the ticket passed from the Open status to the Closed status) - in this example, it is 5,400 business minutes or 10 business days (60 minutes * 9 hours * 10 days);

This robot is flexible and takes into account factors that are not provided for in Atlassian Jira:
- Individual work schedule for each employee (EmployeesData.xlsx),
- List of holidays and holidays (HolidayDates.xlsx).

Moreover, if You don't use Orchestrator Queues robot will make DataTable to build a queue. Or don't use Orchestrator Assets for retrieving Jira Login and Password robot will take it from Config.xlsx

The robot makes a request using the Jira API and receives tickets that have been updated in the last N number of days. Then the change history of each ticket is processed. In the course of processing, when an SLA violation is detected, the responsible officer at the time of the violation (Assignee) is sent a letter with detailed information on the violation, indicating the dates of ticket status changes. Freezing of the time counter is also provided when the Waiting status is installed on the ticket.

Information about processed tickets is stored in the Logs.xlsx log and also contains a log of processing the change of tickets in such a way that, when processing the same ticket, the next time a message is not sent again to Assignee when a violation is detected. For example, on day 1, the robot received a list of tickets that were updated in the last 3 calendar days. The next day, when he will receive the next batch of tickets, it may be those that he processed on the first day. A period of 3 days is used to capture the weekend and Friday, when processed on Monday.

The result of the work is the final report with statistics of processing tickets and a histogram in the context of employees for a visual presentation. This report is sent to the department manager.

NOTE:
The folder contains file "CalculateMinutesBetweenDatesActivities.1.0.0.nupkg". Before you run this project, you must install the project dependency "CalculateMinutesBetweenDates" after adding the "User defined package source" (Ctrl + p -> Settings-> add User defined packages) and fill it in.