# sp_udf_hw
CREATE PROCEDURE Factorial
    @n INT,
    @result BIGINT OUTPUT
AS
BEGIN
    IF (@n < 0)
    BEGIN
        SET @result = NULL;
        RETURN;
    END

    DECLARE @i INT = 1;
    SET @result = 1;

    WHILE @i <= @n
    BEGIN
        SET @result = @result * @i;
        SET @i = @i + 1;
    END
END;


CREATE PROCEDURE LazyStudents
    @Count INT OUTPUT
AS
BEGIN
    SELECT s.StudentID, s.FullName
    FROM Students s
    LEFT JOIN BookRentals b ON s.StudentID = b.StudentID
    WHERE b.StudentID IS NULL;

    SELECT @Count = COUNT(*)
    FROM Students s
    LEFT JOIN BookRentals b ON s.StudentID = b.StudentID
    WHERE b.StudentID IS NULL;
END;

CREATE FUNCTION fn_MinPageBooks()
RETURNS TABLE
AS
RETURN
(
    SELECT p.Name AS Publisher,
           MIN(b.PageCount) AS MinPage
    FROM Publishers p
    JOIN Books b ON p.PublisherID = b.PublisherID
    GROUP BY p.Name
);

CREATE FUNCTION fn_PublishersAvgGreaterThanN(@N INT)
RETURNS TABLE
AS
RETURN
(
    SELECT p.Name
    FROM Publishers p
    JOIN Books b ON p.PublisherID = b.PublisherID
    GROUP BY p.Name
    HAVING AVG(b.PageCount) > @N
);

