# User Statistics and Feedback Module for Unlimited Tycoon

## Project Overview
This project is a software module designed for a multiplayer game to collect user statistics, track in-game progress, and facilitate feedback between players and the administration team. The system used to monitor base development, identify unoptimized gameplay stages, and ensure fair play among users.

## Architecture and Technologies
The system is divided into four main components:
* **Client Application:** Developed in Lua.
* **In-game Server:** Developed in Lua.
* **Business Logic Server:** Implemented using Node.js (JavaScript).
* **Database:** Relational database management system using PostgreSQL.

## Core Features

### Player Features
* Save in-game assets, including currency generators, items, and cosmetic buildings.
* Record statistics for completed game stages, including time spent and completion date.
* Submit feedback to administrators containing a text message and a rating.
* Anti-spam protection restricting feedback submission to one message every 10 minutes.

### Administrator Features
* Automatic authorization upon joining the game, granting access to a hidden side menu.
* Review incoming user feedback with the ability to navigate to the author's full statistics by clicking their profile icon.
* Sort and categorize feedback using statuses: "Spam", "Resolved", or "Interesting".
* Manually search and view detailed statistics for any user by entering their nickname.
* Export all database statistics to a server-side spreadsheet file (Microsoft Excel format) for analytics and graph generation.

### Grand Administrator Features
* Secure authorization to the Grand Admin panel via password.
* View the complete list of active system administrators, including their contact information.
* Add new administrators via a dedicated form that includes duplicate name validation.
* Remove administrators, immediately revoking their access to the admin panel.

## Database Structure
The relational database consists of the following connected tables:
* **Players**: Stores player nicknames and current in-game currency balances.
* **Admins**: Contains administrator names, phone numbers, and email addresses.
* **Comments**: Logs player feedback, ratings, dates, assigned administrators, and resolution statuses.
* **Tools**: Stores names of purchased items, acquisition dates, and purchase prices.
* **Buildings**: Contains information regarding purchased buildings and their acquisition dates.
* **Stats**: Records completed stage names, time taken, and the completion dates for users.

## Database SQL command
```sql
CREATE TABLE Players (
    Player_Name VARCHAR(100) NOT NULL PRIMARY KEY,
    Money BIGINT NOT NULL DEFAULT 0
);

CREATE TABLE Admins (
    Admin_Name VARCHAR(100) NOT NULL PRIMARY KEY,
    Phone VARCHAR(50),
    Email VARCHAR(100)
);

CREATE TABLE Comments (
    ID_Comment SERIAL PRIMARY KEY,
    Content VARCHAR(1000),
    Rate INT,
    Date DATE NOT NULL,
    Status VARCHAR(50) NOT NULL,
    Player_Name VARCHAR(100) NOT NULL,
    Admin_Name VARCHAR(100) NOT NULL,
    FOREIGN KEY (Player_Name) REFERENCES Players(Player_Name) ON UPDATE CASCADE ON DELETE CASCADE,
    FOREIGN KEY (Admin_Name) REFERENCES Admins(Admin_Name) ON UPDATE CASCADE ON DELETE CASCADE
);

CREATE TABLE Tools (
    ID_Tool SERIAL PRIMARY KEY,
    Tool_Name VARCHAR(100) NOT NULL,
    Date DATE NOT NULL,
    Price BIGINT NOT NULL,
    Player_Name VARCHAR(100) NOT NULL,
    FOREIGN KEY (Player_Name) REFERENCES Players(Player_Name) ON UPDATE CASCADE ON DELETE CASCADE
);

CREATE TABLE Buildings (
    ID_Building SERIAL PRIMARY KEY,
    Building_Name VARCHAR(100) NOT NULL,
    Date DATE NOT NULL,
    Player_Name VARCHAR(100) NOT NULL,
    FOREIGN KEY (Player_Name) REFERENCES Players(Player_Name) ON UPDATE CASCADE ON DELETE CASCADE
);

CREATE TABLE Stats (
    ID_Stat SERIAL PRIMARY KEY,
    Stage VARCHAR(100) NOT NULL,
    Minutes INT NOT NULL,
    Date_End DATE NOT NULL,
    Player_Name VARCHAR(100) NOT NULL,
    FOREIGN KEY (Player_Name) REFERENCES Players(Player_Name) ON UPDATE CASCADE ON DELETE CASCADE
);
