# Prolog-based Student Course Advisor

An expert system built using **Prolog** that recommends suitable courses to students based on their performance, interests, and career goals.  

---

## 🚀 Features
- Stores student details (name, percentage, interests, goals).  
- Generates personalized course recommendations.  
- Provides tabular reports and student insights.  
- Demonstrates **logic programming** and **AI-based decision-making**.  

---

## 🖥️ Code Example  
% ------------------ Knowledge Base ------------------

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

% Student future goals
goal(alice, researcher).
goal(bob, cloud_engineer).
goal(charlie, security_expert).

% Available courses
course(ai_basics, ai, 70).
course(machine_learning, ml, 75).
course(cloud_computing, cloud, 65).
course(cyber_security, security, 70).
course(data_science, ai, 80).

% Map courses to future goals
course_goal_match(ai_basics, researcher).
course_goal_match(machine_learning, researcher).
course_goal_match(cloud_computing, cloud_engineer).
course_goal_match(cyber_security, security_expert).
course_goal_match(data_science, researcher).

% ------------------ Recommendation Rules ------------------

% Recommend if interest & percentage match
recommend_course(Student, Course) :-
    student(Student),
    interested_in(Student, Interest),
    percentage(Student, Perc),
    course(Course, Interest, MinPerc),
    Perc >= MinPerc.

% Also allow goal-aware match (this may cause duplicate matches without list_to_set)
recommend_course(Student, Course) :-
    student(Student),
    interested_in(Student, Interest),
    goal(Student, Goal),
    percentage(Student, Perc),
    course(Course, Interest, MinPerc),
    Perc >= MinPerc,
    course_goal_match(Course, Goal).

% ------------------ Utilities ------------------

% Collect recommendations and remove duplicates
suggest_courses(Student, Courses) :-
    findall(Course, recommend_course(Student, Course), List),
    list_to_set(List, Courses).

% collect all interests of a student as a set
get_interests(Student, Interests) :-
    findall(I, interested_in(Student, I), List),
    list_to_set(List, Interests).

% safe getter for goal (returns 'none' if no goal)
get_goal(Student, Goal) :-
    ( goal(Student, G) -> Goal = G ; Goal = none ).

% safe getter for percentage (returns 0 if not present)
get_percentage(Student, P) :-
    ( percentage(Student, X) -> P = X ; P = 0 ).

% print list of items as comma separated (no brackets)
print_list([]) :- write('None').
print_list([H]) :- write(H).
print_list([H|T]) :-
    write(H), write(', '), print_list(T).

% ------------------ Print / Table ------------------

% print single student full details
print_student_details(Student) :-
    format('~nStudent: ~w~n', [Student]),
    get_interests(Student, Interests),
    write(' Interests: '), print_list(Interests), nl,
    get_percentage(Student, Perc),
    format(' Marks: ~w~n', [Perc]),
    get_goal(Student, Goal),
    format(' Goal: ~w~n', [Goal]),
    suggest_courses(Student, Courses),
    write(' Suggested Courses: '),
    ( Courses = [] -> write('None') ; print_list(Courses) ),
    nl.

% print summary for all students (verbose block format)
print_all_students :-
    total_students(N),
    format('~nTotal students: ~w~n', [N]),
    format('-----------------------------------------------~n'),
    student(S),
    print_student_details(S),
    fail.
print_all_students :-
    format('-----------------------------------------------~n'),
    true.

% count distinct students
total_students(N) :-
    findall(S, student(S), L),
    list_to_set(L, Set),
    length(Set, N).

% pretty table (single-line per student)
print_table :-
    format('~n-----------------------------------------------------------~n'),
    format('| Student    | Interests           | Marks | Goal               | Suggested Courses                  |~n'),
    format('-----------------------------------------------------------~n'),
    student(S),
    get_interests(S, Interests),
    get_percentage(S, Perc),
    get_goal(S, Goal),
    suggest_courses(S, Courses),
    format('| ~w~t~11+ | ', [S]),
    ( Interests = [] -> write('None') ; format('~w~t~20+', [Interests]) ),
    format(' | ~w    | ~w~t~20+ | ', [Perc, Goal]),
    ( Courses = [] -> write('None') ; print_list(Courses) ),
    nl,
    fail.
print_table :-
    format('-----------------------------------------------------------~n').

% ------------------ CSV Export ------------------

% write a header and all student rows to students_summary.csv
print_csv(FileName) :-
    open(FileName, write, Stream),
    write(Stream, 'Student,Interests,Marks,Goal,SuggestedCourses'), nl(Stream),
    student(S),
    get_interests(S, Interests),
    get_percentage(S, Perc),
    get_goal(S, Goal),
    suggest_courses(S, Courses),
    % convert lists to comma-free strings (join with ';' to avoid CSV conflict)
    list_to_string_with_sep(Interests, ';', IntStr),
    list_to_string_with_sep(Courses, ';', CourseStr),
    format(Stream, '~w,~w,~w,~w,~w~n', [S, IntStr, Perc, Goal, CourseStr]),
    fail.
print_csv(FileName) :-
    % this clause is reached after fail backtracks exhausted
    open(FileName, append, Stream), close(Stream), !.  % ensure file closed

% helper: join list elements into string separated by Sep
list_to_string_with_sep([], _, '').
list_to_string_with_sep([H], _, Str) :-
    atom_string(H, Str).
list_to_string_with_sep([H|T], Sep, Str) :-
    atom_string(H, Hs),
    list_to_string_with_sep(T, Sep, Ts),
    ( Ts = '' -> string_concat(Hs, Ts, Str) ; string_concat(Hs, Sep, Temp), string_concat(Temp, Ts, Str) ).

% ------------------ End of File ------------------







