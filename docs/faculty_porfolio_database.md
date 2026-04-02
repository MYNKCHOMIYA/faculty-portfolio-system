# Faculty Portfolio Database Documentation

### Database Overview
The database is designed to manage a faculty portfolio system. It consists of 10 interconnected tables tracking users, profiles, departments, courses, and various faculty accomplishments (achievements, education, experience, projects, and publications).

---

### Table Relationships
* **users** (1) to (1) **profiles**: A user account is linked to a faculty profile.
* **departments** (1) to (Many) **courses**: A department can offer multiple courses.
* **departments** (1) to (Many) **profiles**: A department can have multiple faculty members.
* **profiles** (1) to (Many) **achievements**: A faculty member can have multiple achievements.
* **profiles** (1) to (Many) **education**: A faculty member can have multiple educational backgrounds.
* **profiles** (1) to (Many) **experience**: A faculty member can have multiple past experiences.
* **profiles** (1) to (Many) **faculty_courses**: A faculty member can teach multiple courses.
* **courses** (1) to (Many) **faculty_courses**: A course can be taught by multiple faculty members across semesters.
* **profiles** (1) to (Many) **projects**: A faculty member can lead or participate in multiple projects.
* **profiles** (1) to (Many) **publications**: A faculty member can author multiple publications.

---

### Table Data Dictionaries

#### 1. `users`
Stores system authentication and basic user account details.
* **id**: `uuid` (Primary Key, Default: `uuid_generate_v4()`)
* **email**: `character varying(255)` (Unique, Not Null)
* **password_hash**: `character varying(255)` (Not Null)
* **role**: `character varying(20)`
* **created_at**: `timestamp without time zone` (Default: `CURRENT_TIMESTAMP`)

#### 2. `profiles`
Stores detailed information about faculty members.
* **id**: `uuid` (Primary Key, Default: `uuid_generate_v4()`)
* **user_id**: `uuid` (Foreign Key referencing `users.id`)
* **department_id**: `uuid` (Foreign Key referencing `departments.id`)
* **full_name**: `character varying(255)`
* **designation**: `character varying(100)`
* **bio**: `text`
* **phone**: `character varying(20)`
* **office_location**: `character varying(100)`
* **avatar_url**: `character varying(255)`
* **google_scholar_link**: `character varying(255)`
* **orcid**: `character varying(100)`
* **is_verified**: `boolean` (Default: `false`)

#### 3. `departments`
Stores academic department or college classifications.
* **id**: `uuid` (Primary Key, Default: `uuid_generate_v4()`)
* **name**: `character varying(100)` (Not Null)
* **college**: `character varying(100)`
* **created_at**: `timestamp without time zone` (Default: `CURRENT_TIMESTAMP`)

#### 4. `courses`
Stores the catalog of courses available in the institution.
* **id**: `uuid` (Primary Key, Default: `uuid_generate_v4()`)
* **course_code**: `character varying(20)`
* **course_name**: `character varying(255)`
* **department_id**: `uuid` (Foreign Key referencing `departments.id`)
* **credits**: `integer`

#### 5. `faculty_courses`
Junction table tracking which courses are taught by which faculty members per semester.
* **id**: `uuid` (Primary Key, Default: `uuid_generate_v4()`)
* **profile_id**: `uuid` (Foreign Key referencing `profiles.id`)
* **course_id**: `uuid` (Foreign Key referencing `courses.id`)
* **semester**: `character varying(20)`
* **year**: `integer`

#### 6. `achievements`
Stores awards, honors, and other recognitions received by faculty members.
* **id**: `uuid` (Primary Key, Default: `uuid_generate_v4()`)
* **profile_id**: `uuid` (Foreign Key referencing `profiles.id`)
* **title**: `character varying(255)`
* **description**: `text`
* **awarded_date**: `date`
* **issuing_organization**: `character varying(255)`

#### 7. `education`
Stores the academic background and degrees earned by faculty members.
* **id**: `uuid` (Primary Key, Default: `uuid_generate_v4()`)
* **profile_id**: `uuid` (Foreign Key referencing `profiles.id`)
* **degree**: `character varying(100)`
* **field_of_study**: `character varying(100)`
* **university**: `character varying(255)`
* **start_year**: `integer`
* **end_year**: `integer`

#### 8. `experience`
Stores the professional and academic work history of faculty members.
* **id**: `uuid` (Primary Key, Default: `uuid_generate_v4()`)
* **profile_id**: `uuid` (Foreign Key referencing `profiles.id`)
* **institution**: `character varying(255)`
* **position**: `character varying(100)`
* **start_date**: `date`
* **end_date**: `date`

#### 9. `projects`
Stores information about research projects and grants.
* **id**: `uuid` (Primary Key, Default: `uuid_generate_v4()`)
* **profile_id**: `uuid` (Foreign Key referencing `profiles.id`)
* **title**: `character varying(255)`
* **description**: `text`
* **funding_agency**: `character varying(255)`
* **grant_amount**: `numeric`
* **start_date**: `date`
* **end_date**: `date`
* **status**: `character varying(20)`

#### 10. `publications`
Stores research papers, articles, and other academic works.
* **id**: `uuid` (Primary Key, Default: `uuid_generate_v4()`)
* **profile_id**: `uuid` (Foreign Key referencing `profiles.id`)
* **title**: `character varying(255)`
* **authors**: `text`
* **journal_conference**: `character varying(255)`
* **publication_type**: `character varying(50)`
* **publish_date**: `date`
* **doi**: `character varying(100)`
* **abstract**: `text`
* **document_url**: `character varying(255)`