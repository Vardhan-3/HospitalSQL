--1. Query: Find the total amount spent by each patient
SELECT 
    P.Name AS Patient_Name,
    SUM(B.Total_cost + (D.Price * P.Amount)) AS Total_Amount_Spent
FROM 
    PATIENT P
JOIN 
    BILL B ON P.Patient_ID = B.Patient_ID
JOIN 
    PRESCRIBES PR ON P.Patient_ID = PR.Patient_ID
JOIN 
    DRUG D ON PR.Drug_ID = D.Drug_ID
GROUP BY 
    P.Patient_ID;

--2. Query: List all doctors working in a particular department with their specializations
SELECT 
    D.Name AS Doctor_Name,
    SP.Name AS Specialization_Name
FROM 
    DOCTOR D
JOIN 
    WORKS_FOR WF ON D.Doctor_ID = WF.Doctor_ID
JOIN 
    DEPARTMENT DEPT ON WF.Dept_ID = DEPT.Dept_ID
JOIN 
    SPECIALIZES S ON D.Doctor_ID = S.Doctor_ID
JOIN 
    SPECIALIZATION SP ON S.Spec_ID = SP.Spec_ID
WHERE 
    DEPT.Dept_name = 'Operating Theatre';
--3. Query: Find patients who were prescribed a specific drug
SELECT 
    P.Name AS Patient_Name,
    D.Name AS Doctor_Name,
    PR.Prescription_date AS Prescription_Date
FROM 
    PRESCRIBES PR
JOIN 
    PATIENT P ON PR.Patient_ID = P.Patient_ID
JOIN 
    DOCTOR D ON PR.Doctor_ID = D.Doctor_ID
JOIN 
    DRUG DR ON PR.Drug_ID = DR.Drug_ID
WHERE 
    DR.Name = 'Ibuprofen';

--4. Query: Find the total number of treatments performed in each department
SELECT 
    DEPT.Dept_name AS Department_Name,
    COUNT(T.Treatment_Date) AS Number_Of_Treatments
FROM 
    TREATMENT T
JOIN 
    SERVICE S ON T.Service_ID = S.Service_ID
JOIN 
    DEPARTMENT DEPT ON S.Dept_ID = DEPT.Dept_ID
GROUP BY 
    DEPT.Dept_name;

--5. Query: Get the average age of patients who had heart surgery
SELECT 
    AVG(P.Age) AS Average_Age
FROM 
    TREATMENT T
JOIN 
    SERVICE S ON T.Service_ID = S.Service_ID
JOIN 
    PATIENT P ON T.Patient_ID = P.Patient_ID
WHERE 
    S.Description = 'Heart Surgery';

--6. Query: List of patients who have never been assigned to a department
SELECT 
    P.Name AS Patient_Name
FROM 
    PATIENT P
LEFT JOIN 
    ASSIGNED_TO A ON P.Patient_ID = A.Patient_ID
WHERE 
    A.Dept_ID IS NULL;
--7. Query: Get all drugs available in a specific pharmacy
SELECT 
    DR.Name AS Drug_Name
FROM 
    PROVIDES P
JOIN 
    DRUG DR ON P.Drug_ID = DR.Drug_ID
JOIN 
    PHARMACY PH ON P.Pharm_ID = PH.Pharm_ID
WHERE 
    PH.Pharm_loc = 'C WING';

--8. Query: Find doctors with more than 5 years of experience in a specialization
SELECT 
    D.Name AS Doctor_Name,
    SP.Name AS Specialization_Name,
    S.Years_exp AS Years_Of_Experience
FROM 
    SPECIALIZES S
JOIN 
    DOCTOR D ON S.Doctor_ID = D.Doctor_ID
JOIN 
    SPECIALIZATION SP ON S.Spec_ID = SP.Spec_ID
WHERE 
    S.Years_exp > 5;

--9. Query: Find the total number of patients treated by a specific doctor
SELECT 
    D.Name AS Doctor_Name,
    COUNT(DISTINCT T.Patient_ID) AS Number_Of_Patients_Treated
FROM 
    TREATMENT T
JOIN 
    DOCTOR D ON T.Doctor_ID = D.Doctor_ID
WHERE 
    D.Name = 'Gregory House'
GROUP BY 
    D.Doctor_ID;

