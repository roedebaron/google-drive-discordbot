<p align="center">
  <img width="100" src="https://pnggrid.com/wp-content/uploads/2021/05/Discord-Logo-Circle-768x768.png">  
  <img width="170" src="https://logos-world.net/wp-content/uploads/2020/11/Google-Drive-Logo.png">
</p>


<h1 align="center">Google Drive Discord Bot</h1>

<p align="center">
  <img src="https://img.shields.io/badge/C%23-8.0-blue" alt="csharp" style="max-width:100%;"> 
  <img src="https://img.shields.io/badge/.NET Core-3.1-blue" alt="dotnet" style="max-width:100%;"> 
  <img src="https://img.shields.io/badge/discord--net--labs-v3.1.7-blue" alt="discord-net" style="max-width:100%;">
  <img src="https://img.shields.io/badge/google--api--drive-v3-green" alt="drive" style="max-width:100%;"> 
  <img src="https://img.shields.io/badge/google--api--sheets-v4-green" alt="sheets" style="max-width:100%;"> 
  <img src="https://img.shields.io/badge/IDE-VS2019-purple" alt="ide" style="max-width:100%;">
</p>

Dockerized Discord bot providing commands to create and query documents in Google Drive. Extra features include attendance registration and calendar creation using Google Sheets.

## About

The Discord bot is developed using a microservice architecture, but the google-drive-service can also be used independently as a REST API instead of through the Discord GUI/google-drive-bot.

### Microservice: google-drive-bot

Responsible for handling commands recieved from the Discord GUI client.

The bot references the TemplateBot --INSERT LINK

#### Commands

##### Attendance registration

`!attendance` - Display attendance information for this guild

`!attendance all` - Register attendance setting all users as Present

`!attendance all vc` - Register attendance setting all users in a voice channel as Present

`!attendance all but <user_mention1>, <user_mention2>, ...` - Register attendance setting all users as present except for the specified users
- Example usage: `!attendance all but @JohnDoe, @DoeJohn, @JohnJohn`

`!attendance <attendance_registration_time>` - Set time for attendance registration for all users in a voice channel
- Time must be a later time that day and in format `hh:mm`
- Example usage:`!attend 09:45`

`!attendance breakfast <user_mention>` - Register breakfast reminder in attendance sheet for a user


--INSERT IMAGES OF EXAMPLE COMMANDS


### Microservice: google-drive-service

RESTful web service responsible for communicating with Google Drive API. 

The internal architecture is based on the MVC-pattern following the principles from Domain-Driven Design (DDD). 

A brief overview of the layers:

**Core**: Business logic. Represents domain objects and services. 

**View**: I/A

**Controller**: Recieves incoming requests which are delegated to **Core**. The result from Core is returned to the client.

**Infrastructure**: Handles communication with external web services (Google API). No data persistence. 

#### Standalone usage

> TODO:
> - Maybe ref to standalone readme...


Go to `localhost:[PORT]/swagger/v1/swagger.json` to get an overview of the endpoints.

### Drive - Queries

### Sheets - Attendance registration

Registrering attendance requires that: 
- The Google Sheets API is enabled for your GCP service account.
- An url to a spreadsheet where the GCP account has _editor_ access. As such, you have to ensure that the spreadsheet is shared with the email of the account.   
- The sheet complies with certain rules in order for the GoogleDriveService to insert/register the attendance correctly in the sheet. It is possible to have a compliant sheet created for you from a template, by simply providing an id on a completely empty sheet at the first attendance request. Then the service will create an attendance sheet and register the attendance. attendance ready-made 

To register attendance call the endpoint `sheet/attendance-updates` with a json object containing the spreadsheet id and attendees with their presence status and an optional message. If the spreadsheet contains multiple tabs (sheets), and the attendance is **not** on the first tab, then the sheetId must be specified.

```JSONC
{
  "attendees": [
    {
      "name": "Person1",
      "presence": "Present" //  Or use integer 0
    },
        {
      "name": "Person2",
      "presence": "Late" // Or use integer 1
    },
        {
      "name": "Person3",
      "presence": "Absent", // Or use integer 2
      "attendancemessage" : "On vacation" // Optional attendance message
    }
  ],
  "id": {
    "spreadSheetId": "DwaypOAqJclqPmSlWgPAfgxMjaYhLxJDDtyabvaDTIUHqhoc65",
    "sheetId": 123456789 // Optional. Defaults to the first sheet/tab of the specified spreadsheet.
  }
}
```


## Getting Started

Using Google's APIs requires a Google Cloud Platform (GCP) account. You might want to familiarize yourself with the respective API quotas before getting started.

### Configuration

Obtain Google credentials (json-file) for the Google Drive API and Google Sheets API and place the file in the folder `google-drive-service/GoogleDriveService.Infrastructure/Secrets`.

### Running the bot

Either run the services from source or use the provided `docker-compose.yml` file to run it via Docker Compose (recommended).  

Create a root folder and clone this project and the [discord-bot-template](https://github.com/roedebaron/microservice-discord-bot-template) project dependency: 
1. `mkdir my-discord-bot`
2. `git clone https://github.com/roedebaron/google-drive-service.git`
3. `git clone https://github.com/roedebaron/microservice-discord-bot-template`

#### Running from source
2. Run `dotnet run --project GoogleDriveService.Api`. This will automatically download all dependencies, build the project and then run the service. 
3. If no other port has been specified in the configuration, the service is now running on port 5000. 


#### Running with Docker Compose ????
2. Examine the configuration in the `docker-compose.yml` file. Change the Docker host port in the port mapping to your preference or leave as is. 
3. Run `docker-compose up` in the root folder to build and run the service.

> TODO: 
> - Using your own Google API credentials + how to get these.

### Usage

> TODO:
> - Insert screenshot, demo gif of commands...


## Project TODO
- [ ] Push code to repo
- [ ] Better error handling
- [ ] Migrate to .NET 6/C#10 if possible

## Disclaimer



