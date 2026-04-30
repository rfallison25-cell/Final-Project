An Overview and Technical Preview of Our True-Scale Impact Simulation Lab

At first glance, a golf swing appears to be a simple application of physics. You swing and the club strikes a ball, and the ball flies through the air until gravity brings it back to the turf. However, this simple minded view completely ignores the molecular displacement that occurs in the fraction of a millisecond during impact. To bridge this massive gap between atomic scale deformation and actual trajectory, we created ATOM-STRUC v3.1. This project is a fully interactive, web-based, 3D physics engine and 2D flight mapping tool. We designed this to visualize the impact and velocity from different club selections. We wanted to see exactly how kinetic energy transfers through a molecular lattice and how aerodynamics influence the flight.

The Vision and Significance

The main goal of this project was to build a learning tool that works directly in a browser without needing extra physics software. We aimed to build a realistic scaled environment which is often difficult to do while keeping mathematical accuracy. In our 3D design, the environment scales with our input, making 1 unit of space represent 1 yard of real-world distance. The importance of our project is in its dual-view approach. We created it to where there was a 3D impact lab and also a physics & flight path view. This gave us the ability to see the difference in impacts and how it directly impacts the flight of a ball.



<img width="1903" height="905" alt="Screenshot 2026-04-29 194110" src="https://github.com/user-attachments/assets/ee1ce8f1-57c6-4de2-a71e-5ae7b5191bf7" />

Step 1: Applying Foundational Kinematics
The foundation of our project relied on understanding kinematics and also classical mechanics. Before writing a single line of 3D code, we decided to get a good understanding by going out golfing on symposium day.
After, we went and hit the lab. We decided to make our ball be hit from a vacuum. In a vacuum, a launched projectile follows a perfect parabola issued by its initial velocity. The launch angle was dictated by the club's loft, and also the constant downward acceleration of gravity. Using standard kinematic equations, distance can be calculated if the initial vectors are known.  However, we wanted to ensure we could put any MPH we wanted and it would resemble directly to realistic golf yardages. To achieve this, we mapped specific club lofts and assigned distance multipliers for each club. By calculating the necessary x, y, and z velocity vectors based on the club's angle, we ensured that the moment the ball detached from the clubface, it followed gravity's laws.



<img width="1337" height="626" alt="Screenshot 2026-04-29 194222" src="https://github.com/user-attachments/assets/0d9ebec0-232c-4dc9-b00d-ad77ae85fbaa" />

Step 2: Engineering the Molecular Dynamics (Soft-Body Physics)

The most ambitious step of ATOM-STRUC was visualizing the ball not as a rigid sphere, but as a dense lattice of thousands of individual polymer chains. We utilized Three.js to generate a sphere composed of 3,500 distinct atomic particles. Instead of making the atoms bounce off each other, we created a shockwave algorithm. When the clubhead's x-coordinate intersects the ball's radius, the impact state triggers. We went and calculated a push distance to flatten the atoms against the invisible boundary of the clubface. Simultaneously, a wave radius expands outward from an epicenter at the point of impact. We calculated each atom's distance to the expanding wave, applying an intensity and decay modifier based on the inverse-square law of energy dissipation. To translate this data into visual feedback, we implemented a color system. As the kinetic force acts on an atom, its stress level spikes, shifting its material color from a neutral slate to red. As the wave passes and the atom snaps back to its original position. This allowed us to actually watch the kinetic energy ripple through the ball's core.




<img width="1892" height="929" alt="Screenshot 2026-04-29 194130" src="https://github.com/user-attachments/assets/2fbecaf0-8422-430c-bf85-9abecb6c9283" />




Step 3: Aerodynamics and the Magnus Effect.

While the 3D lab focuses on the small scale impact, the second part of our project involved larger level flight physics. To demonstrate a more realistic flight of a ball we built a SVG based trajectory graph. This graph just uses the data from the simulation and makes a visual aid reference. To do this we had to calculate the drag and lift of a golf ball. Drag acts against the ball’s velocity, growing exponentially with speed. Lift acts perpendicular to the flight path. Because a golf club creates massive backspin on the ball, the dimples grab air which makes a low pressure zone that is above the ball and generates lift, much like a plane wing would. This is known as the Magnus effect on a ball. By using these forces we were able to make an accurate trajectory for the flight of a ball. It climbs steeply, hangs, and lastly drops at a sharp angle. We designed the graph to change dynamically based on our input, mapping 4 pixels to each yard of distance, ensuring the scale stayed consistent over multiple trials.




<img width="1579" height="757" alt="Screenshot 2026-04-29 194315" src="https://github.com/user-attachments/assets/7c095893-7d52-4dff-a0f9-1203d83b1cd7" />




Step 4: Bringing it Together with Modern UI
The final step was to make it actually look functional and nice. There are complex calculations in a very easy to use website called Tailwind CSS that kept the modern look we were going for. We even included small but great features like the time scaler. This let us slow down the simulation to watch the atomic compression in slow motion, and also in real time. Another feature was the data readouts for analysing the peak stress and carry yardage as well. By carefully managing the state of the simulation we ensured the application feels stable and also useful.



<img width="1885" height="918" alt="Screenshot 2026-04-29 194210" src="https://github.com/user-attachments/assets/477e1aa7-a8a3-416a-a285-9c665ea304e0" />




