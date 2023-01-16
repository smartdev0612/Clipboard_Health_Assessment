# Ticket Breakdown

We are a staffing company whose primary purpose is to book Agents at Shifts posted by Facilities on our platform. We're working on a new feature which will generate reports for our client Facilities containing info on how many hours each Agent worked in a given quarter by summing up every Shift they worked. Currently, this is how the process works:

- Data is saved in the database in the Facilities, Agents, and Shifts tables
- A function `getShiftsByFacility` is called with the Facility's id, returning all Shifts worked that quarter, including some metadata about the Agent assigned to each
- A function `generateReport` is then called with the list of Shifts. It converts them into a PDF which can be submitted by the Facility for compliance.

## You've been asked to work on a ticket. It reads:

**Currently, the id of each Agent on the reports we generate is their internal database id. We'd like to add the ability for Facilities to save their own custom ids for each Agent they work with and use that id when generating reports for them.**

Based on the information given, break this ticket down into 2-5 individual tickets to perform. Provide as much detail for each ticket as you can, including acceptance criteria, time/effort estimates, and implementation details. Feel free to make informed guesses about any unknown details - you can't guess "wrong".

You will be graded on the level of detail in each ticket, the clarity of the execution plan within and between tickets, and the intelligibility of your language. You don't need to be a native English speaker, but please proof-read your work.

## Your Breakdown Here

1. Design tables in the db (20 mins)

-- facilities
facility_id: integer, autoincreased
facility_name: string

-- agents
agent_id: integer, autoincreased
agent_name: string
customer_id: integer

-- shifts
shift_id: integer, autoincreased
facility_id: integer
agent_id: integer
customer_id: integer
time_spent: integer
created_datetime: timestamp

2. Create API function for getShiftsByFacility (20 mins)
   function getShiftsByFacility(facility_id = 0, quarter = 0) {
   switch (quarter) {
   case 0: // 1st quarter
   start_date = '2023-01-01';
   end_date = '2023-03-31';
   break;
   case 1: // 2nd quarter
   start_date = '2023-04-01';
   end_date = '2023-06-30';
   break;
   case 2: // 3rd quarter
   start_date = '2023-07-01';
   end_date = '2023-09-30';
   break;
   case 3: // 4th quarter
   start_date = '2023-10-01';
   end_date = '2023-12-31';
   break;
   }

data = [];

if (facility_id > 0) {
sql = 'SELECT a.customer_id, a.agent_id, a.agent_name, f.facility_name, SUM(time_spent) as time_spent
FROM shifts s
LEFT JOIN facilities f ON s.facility_id = f.facility_id
LEFT JOIN agents a ON s.agent_id = a.agent_id
WHERE s.facility_id = :facility_id AND s.created_datetime BETWEEN :start_date AND :end_date';

    // excuete the query and set data

}

return data;
}

3. Create API function for generateReport (20 mins)
   function generateReport(facility_id = 0, start_date = "", end_date = "") {
   sql = 'SELECT a.customer_id, s.agent_id, a.agent_name, f.facility_name, s.time_spent, s.facility_id
   FROM shifts s
   LEFT JOIN facilities f ON s.facility_id = f.facility_id
   LEFT JOIN agents a ON s.agent_id = a.agent_id
   WHERE s.shift_id > 0 ';
   if (facility_id > 0) {
   sql += ' AND s.facility_id = :facility_id ';
   }

   if (start_date != '') {
   sql += ' AND s.created_datetime > :start_date';
   }

   if (end_date != '') {
   sql += ' AND s.created_datetime < :end_date';
   }

   // excuete the query and return data
   }

4. Create two view pages, retreive data from controller functions (1 hour)

-- Use Next.js
-- Create two pages, ShiftsByFacility, GenerateReport
-- Use getInitialProps for server side rendering
-- Use axios to retreive data from the api
-- Show data using datatable

5. Integrate PDF generate library for generateReport (30 mins)
