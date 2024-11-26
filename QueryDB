--1. How many patients are there in the system?
SELECT COUNT(*) AS Total_Patients
FROM PATIENT;

--2. What are the names of the doctors working in the 'Anestesiology' department?
SELECT D.Name
FROM DOCTOR D
JOIN WORKS_FOR W ON D.Doctor_ID = W.Doctor_ID
JOIN DEPARTMENT DEP ON W.Dept_ID = DEP.Dept_ID
WHERE DEP.Dept_name = 'Anestesiology';

--3. What is the total cost of the bill for patient 'John Smith'?
SELECT B.Total_cost
FROM BILL B
JOIN PATIENT P ON B.Patient_ID = P.Patient_ID
WHERE P.Name = 'John Smith';

--4. Which patients have been prescribed 'Ibuprofen'?
SELECT P.Name
FROM PATIENT P
JOIN PRESCRIBES PR ON P.Patient_ID = PR.Patient_ID
JOIN DRUG D ON PR.Drug_ID = D.Drug_ID
WHERE D.Name = 'Ibuprofen';

--5. What are the services provided in the 'Cardiology' department?
SELECT S.Description
FROM SERVICE S
JOIN TREATMENT T ON S.Service_ID = T.Service_ID
JOIN DEPARTMENT DEP ON T.Dept_ID = DEP.Dept_ID
WHERE DEP.Dept_name = 'Cardiology';

--6. Which drug is prescribed most frequently (based on the number of prescriptions)?
SELECT D.Name, COUNT(*) AS Prescription_Count
FROM DRUG D
JOIN PRESCRIBES PR ON D.Drug_ID = PR.Drug_ID
GROUP BY D.Name
ORDER BY Prescription_Count DESC
LIMIT 1;

--7. Which doctors have the most years of experience in their specialization?
SELECT D.Name, MAX(S.Years_exp) AS Max_Experience
FROM DOCTOR D
JOIN SPECIALIZES S ON D.Doctor_ID = S.Doctor_ID
GROUP BY D.Name
ORDER BY Max_Experience DESC
LIMIT 1;

--8. Which patients have been assigned to the 'Intensive Care Unit'?
SELECT P.Name
FROM PATIENT P
JOIN ASSIGNED_TO A ON P.Patient_ID = A.Patient_ID
JOIN DEPARTMENT DEP ON A.Dept_ID = DEP.Dept_ID
WHERE DEP.Dept_name = 'Intensive Care Unit';

--9. How much does a 'Heart Surgery' cost?
SELECT Price
FROM SERVICE
WHERE Description = 'Heart Surgery';

--10. What drugs are provided by the 'A WING' pharmacy?
SELECT D.Name
FROM DRUG D
JOIN PROVIDES P ON D.Drug_ID = P.Drug_ID
JOIN PHARMACY PH ON P.Pharm_ID = PH.Pharm_ID
WHERE PH.Pharm_loc = 'A WING';

--11. Which treatments are associated with 'Knee Reconstruction' surgery?
SELECT T.Ailment, T.Treatment_Date
FROM TREATMENT T
JOIN SERVICE S ON T.Service_ID = S.Service_ID
WHERE S.Description = 'Knee Reconstruction';

--12. Who is the doctor who specializes in 'Cardiology'?

SELECT D.Name
FROM DOCTOR D
JOIN SPECIALIZES S ON D.Doctor_ID = S.Doctor_ID
JOIN SPECIALIZATION SP ON S.Spec_ID = SP.Spec_ID
WHERE SP.Name = 'Cardiology';

--13. Which patients have a bill due on or after '2020-01-01'?
SELECT P.Name, B.Due_date
FROM BILL B
JOIN PATIENT P ON B.Patient_ID = P.Patient_ID
WHERE B.Due_date >= CAST('2020-01-01' AS DATE);

--14.Find the total bill amount for each patient, including the services they've received.
SELECT P.Name, SUM(B.Total_cost + S.Price) AS Total_Bill_Amount
FROM BILL B
JOIN PATIENT P ON B.Patient_ID = P.Patient_ID
JOIN INCLUDES I ON B.Bill_ID = I.Bill_ID
JOIN SERVICE S ON I.Service_ID = S.Service_ID
GROUP BY P.Name;

--15.Get the doctor who has prescribed the most drugs (based on prescription counts).
SELECT D.Name, COUNT(PR.Drug_ID) AS Prescription_Count
FROM DOCTOR D
JOIN PRESCRIBES PR ON D.Doctor_ID = PR.Doctor_ID
GROUP BY D.Name
ORDER BY Prescription_Count DESC
LIMIT 1;

--16.Find the average cost of services provided in each department.
SELECT DEP.Dept_name, AVG(S.Price) AS Avg_Service_Cost
FROM SERVICE S
JOIN TREATMENT T ON S.Service_ID = T.Service_ID
JOIN DEPARTMENT DEP ON T.Dept_ID = DEP.Dept_ID
GROUP BY DEP.Dept_name;


--STORED PROCEDURES
--Let's say you want to create a stored procedure that automatically calculates the total bill for a patient whenever a new bill is inserted into the BILL table. This procedure will sum the costs of services associated with the bill and update the Total_cost column in the BILL table.

DELIMITER $$

CREATE PROCEDURE CalculateTotalBill(IN patient_id VARCHAR(20), IN bill_id VARCHAR(20))
BEGIN
    DECLARE total_service_cost DECIMAL(10,2) DEFAULT 0;
    
    -- Calculate the total cost of services associated with the bill
    SELECT SUM(S.Price)
    INTO total_service_cost
    FROM SERVICE S
    JOIN INCLUDES I ON S.Service_ID = I.Service_ID
    WHERE I.Bill_ID = bill_id;
    
    -- Update the bill with the total cost
    UPDATE BILL
    SET Total_cost = total_service_cost
    WHERE Bill_ID = bill_id AND Patient_ID = patient_id;
    
    -- Optionally, output the total cost
    SELECT total_service_cost AS Total_Bill_Amount;
END $$

DELIMITER ;

--Usage Example:
-- Calling the stored procedure to calculate and update the total bill
CALL CalculateTotalBill('P00000001', 'B00000001');


--TRIGGER
--create a trigger that automatically adds a record to the ASSIGNED_TO table whenever a new treatment record is added to the TREATMENT table.

DELIMITER $$

CREATE TRIGGER AfterTreatmentInsert
AFTER INSERT ON TREATMENT
FOR EACH ROW
BEGIN
    DECLARE dept_id VARCHAR(20);
    
    -- Get the department associated with the service of the treatment
    SELECT Dept_ID
    INTO dept_id
    FROM SERVICE S
    WHERE S.Service_ID = NEW.Service_ID;
    
    -- Insert a record into ASSIGNED_TO table
    INSERT INTO ASSIGNED_TO (Patient_ID, Dept_ID, Assignment_Date)
    VALUES (NEW.Patient_ID, dept_id, CURRENT_DATE);
END $$

DELIMITER ;

--Usage Example:

-- Inserting a new treatment record
INSERT INTO TREATMENT (Patient_ID, Doctor_ID, Service_ID, Treatment_Description, Treatment_Date)
VALUES ('P00000001', 'D00000002', 'S00000003', 'Patient needed heart surgery', '2024-01-10');
