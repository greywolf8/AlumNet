#  Alumni Tracking System  

An end-to-end **Java Spring Boot + MySQL** based web application for **managing alumni records, fostering communication, and strengthening alumni-student-faculty engagement**. This system allows multiple colleges to create accounts, manage their alumni data in isolated databases, and enable real-time chat between alumni, faculty, and students.  

---

## Features  

###  **Alumni Management**  
- Add, delete, and update alumni profiles.  
- Each college manages its own alumni database (data isolation).  
- Structured relational database with normalized tables.  

###  **Multi-College Support**  
- Each college gets a **separate MySQL schema**.  
- Students/faculty can only access their college’s alumni data.  
- Secure login with role-based authentication.  

###  **User Roles**  
- **Admin (College):** Can create/manage alumni records.  
- **Faculty/Student:** Can search, view, and interact with alumni.  
- **Alumni:** Can update profiles, share job postings, and engage in chats.  

###  **Communication Module**  
- **Global Chat** – Alumni, faculty, and students of a college can share opportunities, discussions, and events.  
- **Private Chat** – One-on-one messaging between alumni and students/faculty.  
- **Job Posting Board** – Alumni can post internships, job opportunities, or guidance messages.  

---

##  Technical Implementation  

### **Backend**  
- **Java Spring Boot** (RESTful API development, modular service-oriented architecture).  
- **Spring Security** (Role-based authentication and authorization).  
- **JPA/Hibernate** for ORM mapping with MySQL.  
- **WebSocket / STOMP** for real-time chat feature.  

### **Database Design (MySQL)**  
- **Separate schema per college** → ensures data isolation.  
- **Tables per college:**  
  - `alumni_profiles` → stores alumni details (ID, Name, Batch, Company, Role, Contact).  
  - `student_faculty_accounts` → stores user login details (with role: student/faculty).  
  - `chats_global` → global messages per college.  
  - `chats_private` → private one-to-one chat history.  

### **Authentication Workflow**  
1. User logs in with email/password.  
2. System verifies credentials from `student_faculty_accounts` of their college’s schema.  
3. Role-based access ensures:  
   - **Student/Faculty:** Can access only their college’s alumni data.  
   - **Admin:** Can manage alumni records.  
   - **Alumni:** Can interact in chat and update profiles.  

### **Chat Module**  
- Implemented using **Spring WebSocket (STOMP over WebSocket)**.  
- **Global Chat:** Broadcasts messages to all logged-in users of a college.  
- **Private Chat:** Peer-to-peer channel between alumni and student/faculty.  

---

##  Tech Stack  

- **Backend:** Java 17, Spring Boot 3.x, Spring Security, Spring WebSocket  
- **Database:** MySQL 8.x  
- **ORM:** Hibernate/JPA  
- **Build Tool:** Maven/Gradle  
- **API Testing:** Postman  
- **Version Control:** Git + GitHub  

---

##  System Architecture  

```
                        ┌─────────────────────┐
                        │       College        │
                        └─────────┬───────────┘
                                  │
                 ┌────────────────┴────────────────┐
                 │                                 │
        ┌────────▼────────┐              ┌────────▼────────┐
        │ Alumni Database │              │ Account Database│
        │ (per college)   │              │ (students/fac)  │
        └────────┬────────┘              └────────┬────────┘
                 │                                 │
       ┌─────────▼─────────┐             ┌─────────▼─────────┐
       │   Spring Boot API │<--REST----->│ Authentication     │
       │   + Security       │             │ + Authorization   │
       └─────────┬─────────┘             └─────────┬─────────┘
                 │                                 │
          ┌──────▼───────┐                 ┌───────▼───────┐
          │ Alumni Mgmt  │                 │ Chat Module    │
          │ CRUD, Search │<----WebSocket-->| Global + Private│
          └──────────────┘                 └────────────────┘
```

---

##  Setup Instructions  

### **1. Clone Repo**  
```bash
git clone https://github.com/your-username/alumni-tracking-system.git
cd alumni-tracking-system
```

### **2. Configure Database**  
- Install MySQL and create a root user.  
- Update `application.properties`:  
```properties
spring.datasource.url=jdbc:mysql://localhost:3306/
spring.datasource.username=root
spring.datasource.password=yourpassword
spring.jpa.hibernate.ddl-auto=update
```

### **3. Build & Run**  
```bash
mvn clean install
mvn spring-boot:run
```

### **4. Access API Endpoints**  
- `POST /auth/login` → Login for student/faculty/alumni.  
- `POST /college/{id}/alumni` → Add alumni profile.  
- `GET /college/{id}/alumni` → Fetch alumni profiles.  
- `WS /chat/global/{collegeId}` → Join global chat.  
- `WS /chat/private/{userId}` → Start private chat.  

---

##  Future Enhancements  
- ✅ **Frontend UI (React/Angular)** for intuitive dashboards.  
- ✅ **Recommendation System** – Suggest mentors/jobs to students.  
- ✅ **Analytics Dashboard** – Alumni employment stats, contribution tracking.  
- ✅ **Mobile App** (Flutter/React Native) for easy access.  

---

##  Contribution  
Contributions, issues, and feature requests are welcome!  

---

##  License  
MIT License – Free to use and modify.  
