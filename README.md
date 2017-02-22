# Altspace Programming Project - TodoMVC Operations

## Instructions

```
cd Project && docker-compose up
```
My deployment is as sane and simple as possible. I chose to use `docker-compose.yml` to create my services, and create a one step deployment.

Services are :
```
todomvc -> django application
db -> postgres db
web -> nginx webserver
dd-agent -> datadog agent 
```

My reasoning behind using datadog for monitoring, was the simplicity of monitoring the state of containers running on the host. Deploying the DD agent container I can get a lot of metrics around my stack. I would have liked to have reported APM metrics from within the todomvc application to be able to create some insights and analysis of the application.

To Do:
- [x] Single command deploy (docker-compose)
- [x] App backed by RDBMS (Postgres)
- [x] App behind Nginx 
- [x] Automated app setup (syncdb, environment variables etc)
- [x] Automated monitoring stack (datadog) 
- [ ] APM for the todomv application
- [ ] Business intelligence and reporting from the APM metrics

Somethings to take note of:
In order to get the migrations to run correctly, I needed to wait for the DB container to be ready before the app container ran. the app container entry point is to run the `/app-entrypoint.sh` script, which bootstraps the app, and starts uwsgi. I encountered issues where compose would start this container before the db container was ready to accept TCP connections. 

The simplest way I could get around this was to create a `sleep 15` statement at the begining of entry point script. This stalled the running of migrations long enough for the DB container to be ready. For this use case, it works fine, however in more complicated deployments, this could most likely cause issues.