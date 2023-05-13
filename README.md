# trajectory-control
Trajectory control of a wheeled robot in Xcos

Code created to support a Linkedin post. https://www.linkedin.com/posts/simone-bertoni-control-eng_trajectory-control-activity-7063090502551670784-wmhE?utm_source=share&utm_medium=member_desktop

Follow me on Linkedin! https://www.linkedin.com/in/simone-bertoni-control-eng/

What do you know about robot trajectory control?

Give me 5 minutes. It's fun. 👇

We'll use a two-wheeled robot to show an approach to trajectory control.

I have shown in previous posts how to model and control such a robot (links in the comments).

The idea is to build an algorithm that generates ψ_deg_des (heading) and u_des (speed) for the inner robot's control loop.

The desired trajectory is defined by the coordinates:

x_traj(t), y_traj(t)

at any point in time. The objective is to keep the robot as close as possible to such coordinates.

Let's find first the heading of the trajectory:

ψ_traj = atan(y_dot_traj, x_dot_traj) * 180/π

and its speed:

u_traj = sqrt(y_dot_traj^2 + x_dot_traj^2)

Now let's find the distance between the robot and the point on the trajectory:

d = sqrt((y_traj(t) - y(t))^2 + (x_traj(t) - x(t))^2)

and the angle between the position of the robot and the position of the point on the trajectory:

α = atan(y_traj(t) - y(t), x_traj(t) - x(t)) * 180/π

Now we need to generate the two setpoints for the control system:

- ψ_deg_des: yaw angle in degrees

- u_des: linear speed in m/s

and we need to do it so that the robot will follow the point on the trajectory.

Regarding ψ_deg_des, the idea is:

- If the robot is far from the point, we aim to the point and request ψ_deg_des = α

- If the robot is on the point, we request the trajectory heading: ψ_deg_des = ψ_traj

- If the robot is in between, we interpolate between α and ψ_traj

Mathematically, we achieve this with the following algorithm:

ψ_deg_des = (1 - a) ψ_traj + a α

where a = sat(k_dst d, 0, 1), with k_dst= 0.2

k_dst = 0.2 means that for d > 5 m --> a = 1 (if the distance is greater than 5 meter we aim at the point on the trajectory).

Regarding the speed demand, we use:

u_des = sat(k_sp d + u_traj, 0, u_max), with u_max = 3 m/s

with this approach we "catch-up" with the trajectory by going faster when we are far from the point.

The slides below show the model in Xcos and the result of the simulation.

The robot follows the trajectory indeed!

I think it's cool, what do you think?

The link to the model in Xcos on GitHub is in the comments.

If you enjoyed this follow me for more tips on control and embedded software engineering.

Hit the 🔔 on my profile to get a notification for all my new posts.

What do you want to know? Ask in the comments!

#controlsystems #embeddedsystems #softwareengineering #embeddedsoftware #controltheory #robotics #github
