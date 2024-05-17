# micro_ros_action_server
This example demonstrates the utilization of ROS actions within micro-ROS on an ESP32 equipped with ESP-IDF and FreeRTOS, communicate with the agent via USB/UART.
## Modify microros configuration
The default microros configuration only supports 1 node, 2 publishers, 2 subscribers, 1 service, 1 client, and 1 history record.
You need to modify the colcon.meta file in the micro_ros_espidf_component directory. Locate the rmw_microxrcedds section and adjust the settings according to your requirements. Since the action server requires 3 service servers and 2 publishers, I will set the number of publishers, subscribers, servers, clients, and history records all to 3. To enable communication via USB/UART, set the transport mode to 'custom'.

```bash
cd ~/esp/Samples/extra_components/micro_ros_espidf_component
nano colcon.meta
```
```
"rmw_microxrcedds": {
            "cmake-args": [
                "-DRMW_UXRCE_XML_BUFFER_LENGTH=400",
                "-DRMW_UXRCE_TRANSPORT=custom",
                "-DRMW_UXRCE_MAX_NODES=1",
                "-DRMW_UXRCE_MAX_PUBLISHERS=3",
                "-DRMW_UXRCE_MAX_SUBSCRIPTIONS=3",
                "-DRMW_UXRCE_MAX_SERVICES=3",
                "-DRMW_UXRCE_MAX_CLIENTS=3",
                "-DRMW_UXRCE_MAX_HISTORY=3"
            ]
        },
```
## Show the result
This example assumes that you have already installed the micro-ROS agent. To proceed, run the micro-ROS agent with these commands.
```bash
source /opt/ros/humble/setup.bash
source ~/uros_ws/install/local_setup.sh
ros2 run micro_ros_agent micro_ros_agent serial --dev /dev/ttyUSB0 -b 115200 -v4
```
Open a new terminal and execute the following command to call the action server.
```bash
source /opt/ros/humble/setup.bash
ros2 action send_goal /fibonacci example_interfaces/action/Fibonacci "{order: 10}"
```
