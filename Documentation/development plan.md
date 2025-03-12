# Eldercare co. - Project Plan

---

## 1. Persona

**Name:** Eldercare co.  
**Background:**  
* A caregiver or family member responsible for the well-being of elderly individuals.  
* Seeks efficient ways to manage and monitor elder care services.

**Key Characteristics:**  
Compassionate and responsible  
* Values detailed, step-by-step guides  
* Seeks community support and reliable information  
* Prefers user-friendly and intuitive interfaces    

---

## 2. UX Flow



---

## 3. Layout and Navigation

* **Navigation Drawer / Bottom Navigation Bar:**  
  + **Home:** Featured DIY projects and recent activity.
  + **Submit Project:** Form for adding new DIY projects.
  + **Forum:** Access to community discussions.
  + **Profile:** User account settings and project contributions.

* **Screen Layouts:**  
  + **Home Screen:** Card-based layout with project previews, images, and brief descriptions.
  + **Project Detail Screen:** Scrollable view with images, text, and interactive review submission.
  + **Submission Screen:** Form layout with input fields, image uploader, and submit button.
  + **Forum Screen:** List of threads with new post button and sorting/filter options.

* **Consistent Navigation:**  
  + Clear back navigation and intuitive UI components for ease of use.

---

## 4. Color Scheme and Visual Style

* **Primary Colors:**  
  + **Rust:** Conveys warmth and creativity.
  + **Olive Green:** Symbolizes growth and natural inspiration.

* **Accent Colors:**  
  + **Beige:** Provides a neutral, clean background.
  + Additional neutral tones for text and subtle UI elements.

* **Visual Style:**  
  + **Practical and Inspiring:** Clean layout with plenty of white space, large images, and intuitive typography.
  + **Consistent Iconography:** Simple and clear icons that match the DIY and creative vibe.
  + **User-Friendly:** Emphasis on clarity and simplicity in design to ensure a smooth user experience.

---

## 5. Entity Relational Database (ERD)

**Key Entities:**

1. **User**
   - `user_id` (Primary Key)
   - `name`

   - `email`

   - `password_hash`

   - `profile_image_url`

   - `date_joined`

2. **DIY_Project**
   - `project_id` (Primary Key)
   - `user_id` (Foreign Key - User)
   - `title`

   - `description`

   - `image_url`

   - `materials_list` (Array/String)
   - `steps` (Array of steps)
   - `approved` (Boolean)
   - `timestamp`

3. **Review**
   - `review_id` (Primary Key)
   - `project_id` (Foreign Key - DIY_Project)
   - `user_id` (Foreign Key - User)
   - `rating` (Numeric)
   - `comment`

   - `timestamp`

4. **Forum_Thread**
   - `thread_id` (Primary Key)
   - `user_id` (Foreign Key - User)
   - `title`

   - `content`

   - `timestamp`

5. **Forum_Reply**
   - `reply_id` (Primary Key)
   - `thread_id` (Foreign Key - Forum_Thread)
   - `user_id` (Foreign Key - User)
   - `comment`

   - `timestamp`

---

## 6. Dataflow

1. **User Authentication & Registration:**
   - **Firebase Authentication** is used to create new users or sign in existing ones.
   - **Dataflow:** User credentials → Firebase Auth → Secure session token.

2. **DIY Project Submission:**
   - **User Action:** Submit new project using the submission form.
   - **Dataflow:**  

     - User inputs (text, images) → Firebase Storage (for images)  
     - Firebase Firestore (for project details) → `approved` flag set to `false`

     - Admin reviews and updates `approved` to `true` .

3. **Project Display & Review:**
   - **Dataflow:**  

     - Firestore retrieves approved DIY projects → Home Screen display.  
     - User reviews submitted → Stored in Firestore under `Review` collection → Project detail screen shows cumulative ratings and comments.

4. **Forum Interaction:**
   - **Dataflow:**  

     - User posts a thread/reply → Stored in Firestore under `Forum_Thread` and `Forum_Reply` collections.
     - Real-time updates via Firebase’s real-time database capabilities allow immediate discussion updates.

5. **Admin Confirmation Workflow:**
   - **Dataflow:**  

     - New submissions trigger notifications (using Cloud Functions) → Admin dashboard accesses pending projects/reviews.
     - On approval, admin updates project’s `approved` flag → Changes reflected in the public display.

---