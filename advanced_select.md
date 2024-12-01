### The report 
SELECT CASE WHEN G.GRADE >= 8 THEN S.NAME 
        ELSE NULL
        END AS NAME,
        GRADE, MARKS
FROM Grades G JOIN Students S ON S.MARKS BETWEEN G.MIN_MARK AND G.MAX_MARK
ORDER BY GRADE DESC,
         CASE 
            WHEN G.GRADE >= 8 THEN NAME
            ELSE NULL
        END ASC;

### Top competitors
SELECT h.hacker_id, h.name 
FROM Hackers h, Submissions s, Challenges c, Difficulty d
WHERE 
  s.hacker_id = h.hacker_id 
  AND
    s.challenge_id = c.challenge_id 
  AND 
    c.difficulty_level = d.difficulty_level
  AND 
    s.score = d.score
GROUP BY h.hacker_id, h.name
HAVING COUNT(h.hacker_id) > 1
ORDER BY COUNT(h.hacker_id) DESC, hacker_id ASC;


### Ollivander's Inventory
SELECT w.id, wp.age, w.coins_needed, w.power 
FROM Wands w
JOIN Wands_Property wp ON w.code = wp.code
WHERE wp.is_evil = 0
AND w.coins_needed = 
    (SELECT MIN(w1.coins_needed) 
     FROM Wands w1
     JOIN Wands_Property wp1 ON w1.code = wp1.code
     WHERE w1.power = w.power
     AND wp.age = wp1.age)
ORDER BY w.power DESC, wp.age DESC;
