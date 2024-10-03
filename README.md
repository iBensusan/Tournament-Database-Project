# Tournament Database Project

## Objective
The goal of this project is to design, implement, and demonstrate a database system for a multi-sport tournament, supporting different sports (Football, Basketball, Tennis). The database includes tables for storing tournament data, teams, players, and their performances, as well as complex functionalities like views, stored procedures, and triggers to manage and process the tournament data efficiently.

## Project Overview
This project simulates a sports tournament database, which includes multiple sports and teams. It covers the creation of tables, insertion of sample data, and implementation of PL/SQL functionalities, such as views, stored procedures, and triggers, to handle specific operations like calculating player statistics and generating tournament schedules.

## Database Design
The database is structured with both normalized and denormalized tables for optimized data retrieval and performance. The design follows the principles of normalization, but strategic denormalization is applied to avoid complex joins and improve query performance.

### Key Components:
- **Tournament Table**: Stores information about tournaments, including name and duration.
- **Team and Player Tables**: Represent teams and players participating in various sports.
- **PlayerStatistics and TeamStatistics**: Track performance metrics like goals, assists, sets won, and more.
- **Sports-Specific Tables**: Each sport (Football, Basketball, Tennis) has a dedicated table for its specific data attributes.

## SQL and PL/SQL Implementation

### 1. Table Creation
The database includes several tables that store data about tournaments, teams, players, and their statistics. Each table is created using SQL, and all necessary primary and foreign keys are included for data integrity.

Example of table creation:

CREATE TABLE Tournament (
    Tournament_ID Float(10) Not Null,
    Tournament_name Varchar(20) Not Null,
    Start_date Date Not NUll,
    End_date Date Not Null,
    Primary key (Tournament_ID)
);


### 2. Data Insertion
Sample data is inserted into the tables to populate the database. The \`INSERT ALL INTO\` query is used for inserting multiple rows at once for efficiency.

Example data insertion:

INSERT ALL
INTO TournamentSport (Tournament_IDSport_ID) VALUES ('0001''01')
INTO TournamentSport (Tournament_IDSport_ID) VALUES ('0001''02')
INTO TournamentSport (Tournament_IDSport_ID) VALUES ('0001''03')
SELECT * FROM dual;


### 3. Views
Views are created to simplify complex queries by combining data from multiple tables. These views present meaningful information to the user, such as schedules and player performance.

Example view:

CREATE VIEW TournamentScheduleView AS
SELECT t.Tournament_Name, s.Sport_Name, sch.Venue, sch.Date_Time
FROM Tournament t
JOIN Schedule sch ON t.Tournament_ID = sch.Tournament_ID
JOIN Sport s ON s.Sport_ID = sch.Sport_ID;


### 4. Stored Procedures
Stored procedures are implemented to automate processes like calculating player statistics or generating team schedules. These procedures take input parameters and perform updates or insertions into the database based on complex logic.

Example procedure:

CREATE OR REPLACE PROCEDURE CalculatePlayerStatistics(
  p_player_id IN PlayerStatistics.Player_ID%TYPE,
  p_sport_id IN PlayerStatistics.Sport_ID%TYPE
) AS
BEGIN
  UPDATE PlayerStatistics
  SET Goal_Scored = (SELECT SUM(Score) FROM Game WHERE Team_ID IN (SELECT Team_ID FROM Team WHERE Sport_ID = p_sport_id) AND Player_ID = p_player_id),
      Points_Scored = (SELECT SUM(Score) FROM Game WHERE Team_ID IN (SELECT Team_ID FROM Team WHERE Sport_ID = p_sport_id) AND Player_ID = p_player_id),
      Assist = (SELECT COUNT(*) FROM Game WHERE Team_ID IN (SELECT Team_ID FROM Team WHERE Sport_ID = p_sport_id) AND Player_ID = p_player_id)
  WHERE Player_ID = p_player_id AND Sport_ID = p_sport_id;
  COMMIT;
END;


### 5. Triggers
Triggers are used to automatically update player statistics whenever a new game is recorded. This ensures that the data remains consistent and up-to-date without manual intervention.

Example trigger:

CREATE OR REPLACE TRIGGER UpdatePlayerStatisticsTrigger
AFTER INSERT ON Game
FOR EACH ROW
BEGIN
  UPDATE PlayerStatistics
  SET Goal_Scored = Goal_Scored + :NEW.Score,
      Points_Scored = Points_Scored + :NEW.Score,
      Sets_Won = Sets_Won + :NEW.Score,
      Assist = Assist + :NEW.Assist
  WHERE Player_ID = :NEW.Player_ID AND Sport_ID = :NEW.Sport_ID;
END;


## How to Run the Project
1. Clone this repository to your local machine.
2. Set up your database (Oracle, MySQL, etc.) and create the tables as outlined in the SQL scripts.
3. Insert the sample data provided.
4. Use the provided stored procedures and triggers for various database operations.

## Conclusion
This project showcases a comprehensive database system for managing a multi-sport tournament. It uses a combination of table structures, views, procedures, and triggers to offer a robust and efficient solution for handling tournament-related data.

