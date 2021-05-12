# Property-BI-Analysis

--Task 1 - A

SELECT p.Name, o.OwnerId FROM OwnerProperty o INNER JOIN Property p ON o.PropertyId = p.Id WHERE o.OwnerId = 1426;

--Task 1 - B

SELECT p.Name, f.CurrentHomeValue FROM OwnerProperty o INNER JOIN Property p ON o.PropertyId = p.Id INNER JOIN PropertyFinance f ON o.PropertyId = f.PropertyId WHERE o.OwnerId = 1426;

--Task 1-C

SELECT p.Name, t.PaymentAmount,t.PaymentFrequencyId, t.StartDate, t.EndDate, CASE WHEN t.PaymentFrequencyid = 1 THEN SUM(t.PaymentAmount * DATEDIFF(WEEK, t.StartDate, t.EndDate)) WHEN t.PaymentFrequencyid = 2 THEN SUM(t.PaymentAmount/2 * DATEDIFF(WEEK, t.StartDate, t.EndDate)) WHEN t.PaymentFrequencyid = 3 AND DAY(t.StartDate) > DAY(t.EndDate) THEN SUM(t.PaymentAmount * DATEDIFF(MONTH, t.StartDate, t.EndDate)) ELSE SUM(t.PaymentAmount * (DATEDIFF(MONTH, t.StartDate, t.EndDate) + 1)) END TotalPayment FROM OwnerProperty o INNER JOIN Property p ON o.PropertyId = p.Id INNER JOIN TenantProperty t ON o.PropertyId = t.PropertyId WHERE o.OwnerId = 1426 GROUP BY p.Name, t.PaymentAmount,t.PaymentFrequencyId, t.StartDate, t.EndDate

--Task 1 - D

SELECT j.JobDescription, s.Status FROM Job j INNER JOIN JobStatus s ON j.JobStatusId = s.Id WHERE JobStatusId = 1 OR JobStatusId = 2

--Task 1 - E

SELECT p.Name, s.Firstname, s.LastName, y.PaymentAmount, CASE WHEN y.PaymentFrequencyId = 1 THEN 'Weekly' WHEN y.PaymentFrequencyId = 2 THEN 'Fortnightly' WHEN y.PaymentFrequencyId = 3 THEN 'Monthly' END Frequency FROM Person s INNER JOIN Tenant t ON s.PhysicalAddressId = t.ResidentialAddress INNER JOIN TenantProperty y ON t.Id = y.TenantId INNER JOIN Property p ON y.PropertyId = p.Id INNER JOIN OwnerProperty o ON p.Id = o.PropertyId WHERE OwnerId = 1426 AND y.IsActive = 1

--Task 2 - Query Dataset

SELECT s.FirstName, s.LastName, o.OwnerId, o.OwnershipStatusId, p.Name as Property_Name, e.Description as Expense, e.Amount as Expense_Amount, e.Date, a.Street, a.Number, p.Bedroom, p.Bathroom, r.Amount as Rental_Payment FROM PropertyExpense e INNER JOIN Property p ON p.Id = e.PropertyId INNER JOIN Address a ON p.AddressId = a.AddressId INNER JOIN PropertyRentalPayment r ON p.Id = r.PropertyId INNER JOIN OwnerProperty o ON p.Id = o.PropertyId INNER JOIN TenantProperty y ON o.PropertyId = y.PropertyId INNER JOIN Tenant t ON t.Id = y.TenantId INNER JOIN Person s ON s.PhysicalAddressId = t.ResidentialAddress WHERE p.Name = 'Property A' AND o.OwnershipStatusId = 1 OR o.OwnershipStatusId = 2
