CREATE TABLE User_n( -- опис користувача
  full_name VARCHAR(100), -- ПІБ користувача
  date_of_birth DATE, -- Дата народження користувача
  gender VARCHAR(50), -- Стать користувача
  email VARCHAR(100), -- email користувача
  technicalemployee VARCHAR(100) -- технічний співробітник, який обслуговує користувача
);

CREATE TABLE Request_noise_level( -- опис запиту на вимірювання шуму
  location_n VARCHAR(100), -- місце проведення вимірювань
  request_time DATE, -- час надсилання запиту
  request_status VARCHAR(50), -- статус запиту
  user_id INT,  -- номер користувача, який надсилає запит
  FOREIGN KEY (user_id) REFERENCES User(user_id)
);


CREATE TABLE Answer_on_request(
    measured_noise_level FLOAT,
    answer_time DATE,
    answer_status VARCHAR(50),
    requestnoiselevel_id INT,
    system_id INT,
    FOREIGN KEY (requestnoiselevel_id) REFERENCES Request_noise_level(requestnoiselevel_id),
    FOREIGN KEY (system_id) REFERENCES System_user(system_id)
);

CREATE TABLE System_user( -- опис системи
  system_id INT PRIMARY KEY, -- номер системи
  measured_noise FLOAT, -- виміряний рівень шуму
  analyze_noise FLOAT, -- аналізований рівень шуму
  provide_recommendation VARCHAR(100), -- рекомендація щодо зменшення шуму
  technicalemployee_id INT,  -- номер технічного співробітника, який обслуговує систему,
  FOREIGN KEY (technicalemployee_id) REFERENCES Technical_employee(technicalemployee_id)
);

CREATE TABLE Technical_employee( -- опис технічного співробітника
  technicalemployee_id INT PRIMARY KEY,  -- номер технічного співробітника,
  position_n VARCHAR(100),  -- посада технічного співробітника,
  salary FLOAT,  -- заробітна плата технічного співробітника,
  login_credentials VARCHAR(100),  -- логин технічного співробітника,
  current_task VARCHAR(100),  -- поточне завдання технічного співробітника,
  system_access_level INT  -- рiвень доступу до системи технiчного спiвробiтника.
);

ALTER TABLE User_n ADD (
    user_id INT PRIMARY KEY
);

ALTER TABLE Request_noise_level ADD (
    requestnoiselevel_id INT PRIMARY KEY
);

ALTER TABLE Technical_employee ADD CONSTRAINT salary
    CHECK ( salary > 0 );

    -- Додавання нового стовпця до таблиці User_n
ALTER TABLE User_n 
  ADD mobile_phone CHAR(14);

  -- Додавання обмеження до стовпця
ALTER TABLE User_n 
  ADD CONSTRAINT mobile_format_check 
  CHECK (REGEXP_LIKE(mobile_phone, '^(\([0-9]{3}\))?[0-9]{3}-[0-9]{4}$'));


