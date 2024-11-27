# Thoughts junkyard
## Description
Thoughts junkyard is a small pet-project developed by me (the only member of this organization 🙂) in training and learning purposes.<br />
It allows user to create account and perform CRUD on posts.

## Architecture
It consists of 5 parts that are meant to be housed in their own docker container:
- **MySQL** – stores data
- **User service** - works with users and sessions
- **Redis** – caches tokens for user service + stores info for sidekiq jobs  
- **Post service** - works with posts
- **Frontend** - serves data to the end-user

You can see the diagram representing this architecure below.<br />
![Architecture diagram](/thoughts_junkyard.svg)

## How to connect
Unfortunately free PaaS services don't allow to deploy all this magnificence and VPS aren't free<br />
But you still can check it out with private tunneling <br />
All containers are hosted localy on my machine which is online most of the time so using radmin vpn should be enough<br />
(You can see up-to-date tunnel connection data in the place you found this repo)

## How to deploy
1. Install MySQL container from official image. (Tested on mysql:8.4.3)
  - Create db called tj_production
  - Create 2 users with all permissions on this db (one for each of services)<br />
Default user names services will look for are user_service and post_service but you are free to choose new ones
2. Install redis container from official image. (Tested on redis:7.4.1)
3. Install user_service container via evilphilin/tj_user_service
4. Install post_service container via evilphilin/tj_post_service
5. Install frontend part of app via evilphilin/tj_frontend

<details> 
  <summary>Here is the list of necessary envs: </summary>
  User service envs:<br />
  RAILS_MASTER_KEY		= <details><summary>Should be received from protected vault and definitely is not under this spoiler </summary>ce004a39bb153e734f09c451388fb966</details><br />
  USER_SERVICE_DATABASE_HOST	=<br />
  USER_SERVICE_DATABASE_PORT	=<br />
  USER_SERVICE_DATABASE_USERNAME	= user_service (default_value)<br />
  USER_SERVICE_DATABASE_PASSWORD	=<br />
  REDIS_URL			= redis://[host]:[port]<br />
  REDIS_JOBS_DB			= /0 (default value)<br />
  REDIS_CACHE_DB			= /1 (default value)<br />
  CORS_ORIGIN_URLS		= http://[host]:[port]<br />
  // Format: url1,url2,url3 ...<br />
  <br />
  Post service envs:<br />
  RAILS_MASTER_KEY		= <details><summary>Should be received from protected vault and definitely is not under this spoiler </summary>fbb89b4d8e1b80ba04eee10f1d2a3ba9</details><br />
  POST_SERVICE_DATABASE_HOST	=<br />
  POST_SERVICE_DATABASE_PORT	=<br />
  POST_SERVICE_DATABASE_USERNAME	= post_service (default_value)<br />
  POST_SERVICE_DATABASE_PASSWORD	=<br />
  USER_SERVICE_URL:		= http://[host]:[port]<br />
  USER_SERVICE_SESSIONS_PATH	= /sessions (default_value)<br />
  CORS_ORIGIN_URLS		= http://[host]:[port]<br />
  // Format: url1,url2,url3 ...<br />

</details>
