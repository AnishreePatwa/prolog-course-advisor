**Project Description**:

Course selection plays a crucial role in shaping a student’s academic and 
professional journey. Traditionally, students rely on faculty advisors, but these 
decisions are often subjective and may not fully align with the student’s 
performance, interests and long-term goals. 
Artificial Intelligence offers a systematic and data-driven way to handle such 
problems by reducing subjectivity and improving decision-making. Prolog, with its 
strong foundation in predicate logic, rule-based reasoning and pattern matching, is 
particularly well-suited for building expert systems such as course advisors. 
A rule-based advisor system can evaluate multiple factors—such as academic 
percentage, preferred subjects and career aspirations—and then apply logical 
inference to recommend the most suitable electives. Such a system not only saves 
time for both students and faculty but also ensures that recommendations are 
consistent, transparent and personalized. 
Furthermore, the integration of AI into education is not limited to course 
recommendation. Universities across the world are adopting intelligent systems for 
tasks such as personalized learning path generation, predicting student dropout risks 
and analyzing performance trends. These applications highlight the growing role of 
AI in transforming education from a one-size-fits-all model to a more adaptive, 
learner-centric approach.

**Problem Statement**:

Students often face difficulty in identifying electives that align with their overall 
profile. The key challenges include: 
➢ Interests (e.g., Artificial Intelligence, Cloud Computing, Information Security). 
➢ Academic performance (measured through percentage/grades). 
➢ Future career goals (e.g., researcher, cloud engineer, cybersecurity analyst). 
To address this, the objective of the project is to develop a Prolog-based 
Student Course Advisor. The system leverages facts, rules and constraints to 
evaluate a student’s profile and recommend the most suitable elective courses. 
By combining logical inference with structured decision-making, the advisor 
ensures that the selected courses are not only academically appropriate but also 
aligned with the student’s long-term aspirations. 

**Sample Code Snippet**:

% Students
student(alice).
student(bob).
student(charlie).

% Student percentage
percentage(alice, 85).
percentage(bob, 60).
percentage(charlie, 75).

% Student interests
interested_in(alice, ai).
interested_in(alice, ml).
interested_in(bob, cloud).
interested_in(charlie, security).

% Courses
course(ai_basics, ai, 70).
course(machine_learning, ml, 75).
course(cloud_computing, cloud, 65).
course(cyber_security, security, 70).
course(data_science, ai, 80).

% Recommendation Rule
recommend_course(Student, Course) :-
    student(Student),
    interested_in(Student, Interest),
    percentage(Student, Perc),
    course(Course, Interest, MinPerc),
    Perc >= MinPerc.

**Sample Query**:

?- recommend_course(alice, Course).
Course = ai_basics ;
Course = machine_learning ;
Course = data_science.

**How to Run:**

Install SWI-Prolog
Clone or download this repository.
Open terminal in the project folder.
Load the Prolog file:
swipl advisor.pl

**Run queries such as:**

?- recommend_course(bob, Course).
?- print_student_details(alice).
?- print_table.
?- print_csv('students_summary.csv').

**Skills Used:**

Prolog (Logic Programming)
Artificial Intelligence
Expert Systems
Knowledge Representation & Reasoning
Problem Solving & Query Handling
