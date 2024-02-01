## Repo
`db-university`

## DB
[[Database/2024-01-31 - DB University|DB University]]

## Todo
Dopo aver testato le vostre query con `phpMyAdmin`, riportatele in un file `txt` o `md` e caricatelo nella vostra repo.

### Query
#### Group by
1. Contare quanti iscritti ci sono stati ogni anno
```sql
SELECT YEAR(enrolment_date), COUNT(*) 
FROM students 
GROUP BY YEAR(enrolment_date);
```

2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio
```sql
SELECT office_address, COUNT(*)
FROM teachers
GROUP BY office_address;
```

3. Calcolare la media dei voti di ogni appello d'esame (dell'esame vogliamo solo l'`id`)
```sql
SELECT exam_id, AVG(vote) 
FROM exam_student 
GROUP BY exam_id;
```

4. Contare quanti corsi di laurea ci sono per ogni dipartimento
```sql
SELECT department_id, COUNT(*) 
FROM degrees 
GROUP BY department_id;
```


#### Join
1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia
```sql
SELECT degrees.name, students.*
FROM degrees
    JOIN students
        ON degrees.id = students.degree_id
WHERE degrees.name LIKE 'Corso di Laurea in Economia';
```

2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze
```sql
SELECT degrees.* 
FROM departments 
JOIN degrees 
ON degrees.department_id = departments.id 
WHERE departments.name LIKE 'Dipartimento di Neuroscienze'
AND degrees.level LIKE 'Magistrale';
```

3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
```sql
SELECT  teachers.id, courses.*
FROM  teachers
    JOIN course_teacher
        ON  teachers.id = course_teacher.teacher_id
    JOIN courses
    	ON course_teacher.course_id = courses.id
WHERE teachers.id = 44;
```

4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome
```sql
SELECT students.*, degrees.*
FROM students
	JOIN degrees
		ON students.degree_id = degrees.id
	JOIN departments
		ON degrees.department_id = departments.id
ORDER BY students.surname, students.name;
```

5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti
```sql
SELECT
    degrees.name, courses.name, teachers.name
FROM
    degrees
JOIN
    courses ON degrees.id = courses.degree_id
JOIN
    course_teacher ON courses.id = course_teacher.course_id
JOIN
    teachers ON course_teacher.teacher_id = teachers.id
ORDER BY
    degrees.name, courses.name, teachers.name;
```

6. Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)
```sql
SELECT
    teachers.*
FROM
    teachers
JOIN
    course_teacher ON teachers.id = course_teacher.teacher_id
JOIN
    courses ON course_teacher.course_id = courses.id
JOIN
    degrees ON courses.degree_id = degrees.id
JOIN
    departments ON degrees.department_id = departments.id
WHERE
    departments.name = 'Dipartimento di Matematica'
GROUP BY
    teachers.id
ORDER BY
    teachers.name;
```


##### Bonus
7. Selezionare per ogni studente il numero di tentativi sostenuti per ogni esame, stampando anche il voto massimo. Successivamente, filtrare i tentativi con voto minimo 18.
```sql
SELECT
    exam_student.student_id,
    exam_student.exam_id,
    COUNT(*) attempts,
    MAX(exam_student.vote) max_vote
FROM
    exam_student
WHERE
    exam_student.vote >= 18
GROUP BY
    exam_student.student_id, exam_student.exam_id;

```