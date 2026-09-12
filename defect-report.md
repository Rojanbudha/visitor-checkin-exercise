1. Defect-01
Defect: Deactivated visitors remain in the active list and also remain in searchable

Type: Functional

Description: Deactivating a visitors only sets their active field to false. While the active visitor list and search endpoint do not consider that field in their module.So a deactivated visitors still shows up in the active list and can be use found fro repeated visit.

Steps To Reproduce:
 1.Register a new visitor via registration form. for eg: 'dipesh poudel',host'Alice Mercer'
 2.Note the visitor's ID from api response from network or via using route (GET /api/visitors).
 3.Deactivate the visitor directly via the curl command: curl -X PATCH http://localhost:3000/api/visitors/<id>/deactivate. Confirm the response shows "active": false.
 4.Refresh the Active Visitors list in the UI
 5.Search for the visitor's name via curl "http://localhost:3000/api/visitors/search?q=dipesh".

Expected Result: After deactivation, the visitor should not appear in the Active Visitors list, and should not appear in search results.

Actual Result: The deactivated visitor (id 83, "dipesh poudel") still appears in the Active Visitor list and is still returned by the search endpoint.


2. Defect-02
Defect: Checked-in times are displayed in UTC instead of local time

Type: Data display

Description: In the frontend part, checked-in is using (.toISOString()), which always return UTC time instead of local. Due to which check-in time for visitors in active visitor table is off by 5 hours 45 minutes compare to local time.

Steps To Reproduce:
 1.Call (GET/api/visitors) routes or use curl command and note visitors Checked_in_at value.
 2.View the same visitor's row in the Active Visitors table in the UI.
 3.Compare the displayed time to the correct Kathmandu local time.

 Expected Result:The UI should display 23:30 (2026-09-10, Kathmandu local time) for a visitor whose checked_in_at is 2026-09-10 17:45:00 UTC.

 Actual Result: The UI dispaly , the same unconverted raw UTC time which is 17:45


3.Defect-03
Defect:  Registering a visitor with no information still creates a record(missing server-side validation).

Type: Data / functional

Description: A person can register with no prior information and it still created a visitor record with empty blank fields. Because in the Visitor model there is no rule for requiring these fields to be filled in and even host is marked as optional.

Steps To Reproduce:
 1.Send an empty registration request:
   curl -X POST http://localhost:3000/api/visitors \
     -H "Content-Type: application/json" \
     -d '{}'.
 2. Check the response.

Expected Result: The request should be rejected with an error saying required fields are missingand no record should be created. 

Actual Result: The request succeeds, and a new visitor record is created with no name, no host, and no other details filled in.



NOte: While going through the backend with the help of anthropic . i found a little off with pagination. i am not sure whether to indicate it as a bug or not .
 what i mean is invalid values are not validated or rejected  for eg: if we search for 
 a. page=0 and page=-1 produce a negative offset,  returns the same data as page=1.
 b. page=abc (non-numeric) still return the same result as page=0.
 it doesn't effect the web page as whole still worth mentioning.
