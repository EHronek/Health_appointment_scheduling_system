# 🩺 Healthcare Appointment Scheduling System

A robust and secure backend system built with **Python**, **Flask**, and **SQLAlchemy ORM** for managing appointments, doctors, patients, and medical records in a healthcare setting. Designed with modularity, scalability, and real-world usability in mind, this system features RESTful APIs, role-based authentication, and a powerful command-line interface (CLI) for managing objects and the entire application lifecycle.

---

# ⚙️ System Architecture
### 🧩 Components
Backend: Flask + SQLAlchemy ORM

Database: MySQL

Auth: OAuth 2.0 (Google), JWT

Frontend: (Optional) React or basic HTML/CSS/JS

---

## 🚀 Features

- 🧠 **Modular Object-Oriented Architecture**
- 🔐 **OAuth 2.0 Integration** — Authenticate users via third-party providers and auto-register them in the database as a system user with name, email and default role as a patient.
- 🔑 **Role-Based Access Control (RBAC)** — Enforced using JWT tokens for secure API access.
- 🩺 **Doctor & Patient Management** — Securely manage records and interactions with other entities effeciently.
- 📅 **Appointment Scheduling System** — Avoid conflicts with real-time doctor availability management.
- 📄 **Medical Record Handling** — Manage confidential medical histories efficiently and only accessible to patient and doctor sharing a common appointment.
- ⚙️ **Custom CLI Tool** (`console.py`) — Full CRUD capabilities via terminal for all models.
- 🔁 **Dynamic Environment Config** — Secure loading of DB credentials and secrets from environment variables.
- 🧱 **Storage Engine Abstraction** — Clean separation of database logic with `db_storage.py` and `file_storage.py`.

---

## 🧠 Project Architecture

healthcare-scheduler/ ├── api/ │ └── v1/ │ ├── routes/ │ └── app.py ├── auth/ │ ├── oauth.py │ └── jwt_handler.py ├── config/ │ └── env.py ├── console.py ├── models/ │ ├── base_model.py │ ├── doctor.py │ ├── patient_record.py │ ├── appointments.py │ ├── availability.py │ ├── exception.py │ ├── medical_record.py │ └── engine/ │ ├── db_storage.py │ └── file_storage.py ├── requirements.txt └── README.md



---

## 🧩 Core Models

| Model            | Description |
|------------------|-------------|
| `BaseModel`       | Parent class for all models (includes ID, timestamps, save/delete logic, to_dict). |
| `Doctor`          | Stores profile data and working availability. |
| `Patient`         | Tracks patient details and links to medical records. |
| `Appointments`    | Manages scheduled meetings between doctors and patients. |
| `Availability`    | Defines when doctors are available for appointments. |
| `MedicalRecord`   | Stores confidential medical history of patients linked to an appointment. |
| `Exception`       | Handles scheduling edge cases (e.g. doctor out-of-office). |

---

## 🧠 Storage Engine Design

The `models/engine/` directory abstracts all database logic:

- `db_storage.py`: 
  - Defines reusable methods like `.all()`, `.get()`, `.new()`, `.save()`, `.delete()`, `.count()`, `.find()`, `.check_user()`, `.get_user_by_email()`, `.reload()`, `.get_session()`, `.close()` etc.
  - Manages the SQLAlchemy session and engine instance.
  - Fetches database credentials securely from environment variables.
  
- `file_storage.py`:
  - Placeholder for future JSON-based storage support.

This design eliminates repetitive CRUD logic and promotes cleaner code throughout the project.

---

## 💻 Command Line Interface — `console.py`

A powerful terminal-based CLI for developers to interact with the system’s data:
Allows quick management (create, update, destroy, etc) objects via a console / command interpreter in the Backend
store and persist objects to a file (JSON file)

### **`NOTE`** PROTOTYPE STILL INCOMPLETE

## Sample Commands to CRUD an object:
```bash
$ ./console.py
(HMS_$)
(HMS_$)

# Create a doctor
(HMS $) all MyModel
** class doesn't exist **
(HMS $) show BaseModel
** instance id missing **
(HMS $) show BaseModel My_First_Model
** no instance found **
(HMS $) create BaseModel
49faff9a-6318-451f-87b6-910505c55907
(HMS $) all BaseModel
["[BaseModel] (49faff9a-6318-451f-87b6-910505c55907) {'created_at': datetime.datetime(2017, 10, 2, 3, 10, 25, 903293), 'id': '49faff9a-6318-451f-87b6-910505c55907', 'updated_at': datetime.datetime(2017, 10, 2, 3, 10, 25, 903300)}"]
(HMS $) show BaseModel 49faff9a-6318-451f-87b6-910505c55907
[BaseModel] (49faff9a-6318-451f-87b6-910505c55907) {'created_at': datetime.datetime(2017, 10, 2, 3, 10, 25, 903293), 'id': '49faff9a-6318-451f-87b6-910505c55907', 'updated_at': datetime.datetime(2017, 10, 2, 3, 10, 25, 903300)}
(HMS $) destroy
** class name missing **
(HMS $) update BaseModel 49faff9a-6318-451f-87b6-910505c55907 first_name "Betty"
(HMS $) show BaseModel 49faff9a-6318-451f-87b6-910505c55907
[BaseModel] (49faff9a-6318-451f-87b6-910505c55907) {'first_name': 'Betty', 'id': '49faff9a-6318-451f-87b6-910505c55907', 'created_at': datetime.datetime(2017, 10, 2, 3, 10, 25, 903293), 'updated_at': datetime.datetime(2017, 10, 2, 3, 11, 3, 49401)}
(HMS $) create BaseModel
2dd6ef5c-467c-4f82-9521-a772ea7d84e9
(HMS $) all BaseModel
["[BaseModel] (2dd6ef5c-467c-4f82-9521-a772ea7d84e9) {'id': '2dd6ef5c-467c-4f82-9521-a772ea7d84e9', 'created_at': datetime.datetime(2017, 10, 2, 3, 11, 23, 639717), 'updated_at': datetime.datetime(2017, 10, 2, 3, 11, 23, 639724)}", "[BaseModel] (49faff9a-6318-451f-87b6-910505c55907) {'first_name': 'Betty', 'id': '49faff9a-6318-451f-87b6-910505c55907', 'created_at': datetime.datetime(2017, 10, 2, 3, 10, 25, 903293), 'updated_at': datetime.datetime(2017, 10, 2, 3, 11, 3, 49401)}"]
(HMS $) destroy BaseModel 49faff9a-6318-451f-87b6-910505c55907
(HMS $) show BaseModel 49faff9a-6318-451f-87b6-910505c55907
** no instance found **
(HMS $)
(HMS $) User.all()
[[User] (246c227a-d5c1-403d-9bc7-6a47bb9f0f68) {'first_name': 'Betty', 'last_name': 'Bar', 'created_at': datetime.datetime(2017, 9, 28, 21, 12, 19, 611352), 'updated_at': datetime.datetime(2017, 9, 28, 21, 12, 19, 611363), 'password': '63a9f0ea7bb98050796b649e85481845', 'email': 'airbnb@mail.com', 'id': '246c227a-d5c1-403d-9bc7-6a47bb9f0f68'}, [User] (38f22813-2753-4d42-b37c-57a17f1e4f88) {'first_name': 'Betty', 'last_name': 'Bar', 'created_at': datetime.datetime(2017, 9, 28, 21, 11, 42, 848279), 'updated_at': datetime.datetime(2017, 9, 28, 21, 11, 42, 848291), 'password': 'b9be11166d72e9e3ae7fd407165e4bd2', 'email': 'airbnb@mail.com', 'id': '38f22813-2753-4d42-b37c-57a17f1e4f88'}]
(HMS $)
(HMS $) User.count()
2
(HMS $)  
(HMS $) User.show("246c227a-d5c1-403d-9bc7-6a47bb9f0f68")
[User] (246c227a-d5c1-403d-9bc7-6a47bb9f0f68) {'first_name': 'Betty', 'last_name': 'Bar', 'created_at': datetime.datetime(2017, 9, 28, 21, 12, 19, 611352), 'updated_at': datetime.datetime(2017, 9, 28, 21, 12, 19, 611363), 'password': '63a9f0ea7bb98050796b649e85481845', 'email': 'airbnb@mail.com', 'id': '246c227a-d5c1-403d-9bc7-6a47bb9f0f68'}
(HMS $) User.show("Bar")
** no instance found **

```


# 🔐 Authentication & Security
**OAuth 2.0 with Google for secure login**:
- Supports third-party login
- Auto-creates a new user in DB if not existing

# JWT-based RBAC:
- Protects API endpoints based on user roles (admin, doctor, patient)
- Validates access tokens for every secure operation
- CSRF protection for forms and crititcal endpoints

# Security Measures
- JWT token expiration + refresh flow
- Role-based access (admin, patient, doctor)
- CSRF tokens for form-based auth
- Input validation on all API endpoints


# API Documentation



# 🛠️ Setup & Usage
**✅ Environment Variables Required:**
```bash
$ export HMS_MYSQL_USER=database_username
$ export HMS_MYSQL_PWD=database_password
$ export HMS_MYSQL_HOST=localhost
$ export HMS_MYSQL_DB=database_name
$ export HMS_TYPE_STORAGE=db/fs
$ export JWT_SECRET_KEY="jwt_Secret_key"
$ export CLIENT_ID="google_api_client_id"
$ export CLIENT_SECRET="google_api_client_secret"
```





