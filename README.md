# Functionality
This repository provide robot controllers which interface gazebo simulator and execute missions using robot in different environmental conditions. Also, it provides utilities to perturb the environment, e.g., place obstacle in the world, set charge for the robot battery.  

# Dependency

```bash
https://github.com/cmu-mars/brass_gazebo_config_manager
https://github.com/cmu-mars/brass_gazebo_battery
https://github.com/cmu-mars/cp1_base
```

# Install

This package uses cp1 battery plugins services, the gazebo packages should be installed:

```bash
git clone https://github.com/cmu-mars/cp1_controllers.git
cd cp1_controllers
make
```

# Usage

After running `roscore` service and launching the robot `roslaunch launch/cp1-base-test.launch`, we can use the `cli` as follows:

```bash
python cli.py execute_task_reactive l2 l3 l4 l5
python cli.py execute_task l2 l3 l4 l5
python cli.py place_obstacle -19.08 11.08
python src/cli.py set_charge 32560.0
python src/cli.py go_directly l1 l2
python src/cli.py remove_obstacle Obstacle_0
```

# Tests

Launch CP1 (change `docker-compose-no-th.yml` accordingly with the required baseline params):

```bash
TH_PORT=8081 TA_PORT=8080 docker-compose -f docker-compose-no-th.yml up
```

Get status: 

```bash
curl -X GET "http://0.0.0.0:8080/observe" -H "accept: application/json"
```

Perturb (place obstacle):

```bash
curl -X POST "http://brass-ta/perturb/place-obstacle" -H "accept: application/json" -H "Content-Type: application/json" -d "{ \"x\": 0, \"y\": 0}"
```

Perturb (remove obstacle):

```bash
curl -X POST "http://brass-ta/perturb/remove-obstacle" -H "accept: application/json" -H "Content-Type: application/json" -d "{ \"obstacleid\": \"string\"}"
```

Perturb (battery set):

```bash
curl -X POST "http://brass-ta/perturb/battery" -H "accept: application/json" -H "Content-Type: application/json" -d "{ \"charge\": 0}"
```

Start:

```bash
curl -X POST "http://0.0.0.0:8080/start" -H "accept: application/json"
```

