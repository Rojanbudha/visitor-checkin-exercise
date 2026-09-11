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
