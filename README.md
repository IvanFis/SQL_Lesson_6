# SQL_Lesson_6
USE lesson_4;

-- Создание таблицы users_old
CREATE TABLE users_old LIKE users;

-- Создание процедуры для перемещения пользователя из таблицы users в таблицу users_old
DELIMITER //
CREATE PROCEDURE move_user(IN user_id INT)
BEGIN
    START TRANSACTION;
    
    INSERT INTO users_old (id, firstname, lastname, email)
    SELECT id, firstname, lastname, email
    FROM users
    WHERE id = user_id;
    
    DELETE FROM users
    WHERE id = user_id;
    
    IF ROW_COUNT() = 1 THEN
        COMMIT;
        SELECT 'Перемещение пользователя в таблицу users_old прошло успешно.';
    ELSE
        ROLLBACK;
        SELECT 'Ошибка выполнения';
    END IF;
END //
DELIMITER ;

-- Создание хранимой функции hello
DELIMITER //
CREATE FUNCTION hello()
RETURNS VARCHAR(30)
BEGIN
    DECLARE current_hour INT;
    DECLARE greeting VARCHAR(30);
    
    SELECT HOUR(NOW()) INTO current_hour;
    
    IF current_hour >= 6 AND current_hour < 12 THEN
        SET greeting = 'Доброе утро';
    ELSEIF current_hour >= 12 AND current_hour < 18 THEN
        SET greeting = 'Добрый день';
    ELSEIF current_hour >= 18 AND current_hour < 24 THEN
        SET greeting = 'Добрый вечер';
    ELSE
        SET greeting = 'Доброй ночи';
    END IF;
    
    RETURN greeting;
END //
DELIMITER ;

