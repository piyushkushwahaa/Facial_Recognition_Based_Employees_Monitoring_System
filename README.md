                                               Employee Registration Module     
 User Inputs (name, Email, Org, EmpID)
         |
         ▼
  [Webcam Capture]
    cv2.VideoCapture
         |
         ▼
  [Face Detection]
    Haar Cascade
         |
         ▼
  [Crop & Resize]
    50×50px → 50 samples
         |
         ▼
  [Reshape & Validate]
    (50, 7500)
         |
    |---------|
   YES        NO → "Shape mismatch!"
    |
    ▼
[Save to data/]
  ├── names.pkl
  ├── faces_data.pkl
  ├── Emails.pkl
  ├── Organizations.pkl
  └── EmployeeIDs.pkl

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
                                           Employee Attendance Registration 

Loading Data from .pkl Files (Faces, Names, IDs, Emails, Orgs)
            │
            ▼
    KNN Model Training
            │
            ▼
   Webcam → Face Detection
            │
            ▼
  Crop → Resize → Normalize
            │
            ▼
   KNN Predict + Threshold
    ┌───────┴───────┐
 Unknown         EmployeeID
                    │
                    ▼
            Lookup (Name, Org)
                    │
                    ▼
          Display on Screen
                    │
               [Press 'o']
                    │
                    ▼
          Log to CSV File
                    │
               [Press 'q' / 10s]
                    │
                    ▼
              Release Camera


---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
                                                                 Employee Monitoring System  

Camera (Webcam)
     │
     ▼
Threaded Frame Reader (CameraStream)
     │
     ▼
Resize to 640px ──▶ YOLOv8 Person Detection
     │
     ▼
For each detected person:
     │
     ├──▶ Face Recognition (KNN + Haar Cascade, every 10th frame)
     │         │
     │         ▼
     │    (Name, EmployeeID, Email)  or  ("People", "N/A", "")
     │
     ├──▶ Desk Overlap Check (bbox vs binary desk mask)
     │         │
     │         ▼
     │    overlap > 40% ──▶ "Inside Desk" (Employee)
     │    overlap ≤ 40% ──▶ "Outside Desk" (Visitor)
     │
     ▼
2s-Second Presence Timer
     │
     ├── Confirmed ──▶ detection(Log to CSV File)
     │
     └── Person disappears (> 3s) ──▶ absence(Log to CSV File)
     │
     ▼
Draw on Frame (bbox + HUD) ──▶ Display
