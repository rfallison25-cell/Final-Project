🧪 Atomic Golf Impact real-time 3D and physics simulation of golf ball impact and flight.This project simulates the full lifecycle of a golf shot, focusing on high-fidelity visual and physical accuracy. It features:Impact deformation (particle-based mechanics)Energy transfer through the ballProjectile motion with real physics constraintsDynamic trajectory visualization.

🚀 Quickstart To get the simulation running locally on your machine:Bashgit clone https://github.com/rfallison25-cell/Final-Project
open index.html



⚙️ Core ArchitectureThe system is split into two synchronized engines that run off the same dynamic inputs:3D Physics Engine (Three.js)2D Trajectory Engine (SVG + math)Both engines are driven by user-defined parameters:

let mph = parseFloat(document.getElementById('impactSpeedMph').value) || 100;
let clubKey = document.getElementById('clubSelect').value;
let clubParams = CLUBS[clubKey];


🧱 Particle-Based Golf BallInstead of a single static mesh, the ball is composed of thousands of individual particles. This creates a dense, realistic structure for calculating deformation.JavaScript

const ATOM_COUNT = 3500;
const BALL_RADIUS = 4.0;

Each particle stores its original position to ensure it can snap back or deform accurately.

atom.userData = { originalPos: atom.position.clone() };
Particles are mathematically distributed in a spherical volume to ensure even density:JavaScriptconst phi = Math.acos(-1 + (2 * i) / ATOM_COUNT);
const theta = Math.sqrt(ATOM_COUNT * Math.PI) * phi;



💥 Impact & Deformation LogicDuring impact, each particle's position is checked against the club face. This logic produces realistic compression, flattening against the club face, and directional force response.

let worldX = ballGroup.position.x + atom.userData.originalPos.x;
let clubFaceX = simData.clubX + 0.8;

if (worldX < clubFaceX) {
    let push = clubFaceX - worldX;
    targetPos.x += push;
}



🌊 Shockwave PropagationEnergy travels through the ball using a radial wave. Force is applied to individual particles based on their distance from the shockwave's epicenter.
simData.waveRadius += 14.0 * dt;

let distToEpicenter = atom.userData.originalPos.distanceTo(epicenter);
let diffFromWave = Math.abs(distToEpicenter - simData.waveRadius);

let intensity = 1.0 - (diffFromWave / waveWidth);
let force = intensity * decay * (simData.mph / 20);


🎨 Stress VisualizationMaterial stress during impact is mapped to color using HSL interpolation, where Blue indicates low stress and Red indicates high stress.

targetColor.setHSL(
  0.6 - (Math.min(stressLevel, 1) * 0.6),
  0.9,
  0.5
);



🚀 Launch Physics & Flight SimulationThe simulation calculates the exact required launch velocity to ensure the ball lands at the correct mathematical distance based on the club loft and speed inputs, The equatin used is

v= (Dg/sin(20)^1/2


Once airborne, motion and gravity are applied continuously per frame.
ballGroup.position.x += simData.ballVel.x * dt;
ballGroup.position.y += simData.ballVel.y * dt;
simData.ballVel.y -= GRAVITY * dt;



🎯 Landing CorrectionTo eliminate floating-point drift and ensure perfect accuracy (and alignment with the 2D graph), the final position is snapped to the exact carry value.

let exactCarry = simData.mph * clubParams.multiplier;
ballGroup.position.x = exactCarry;


📈 Trajectory Graph (SVG)The trajectory is plotted dynamically alongside the 3D simulation to provide analytical feedback.Parabolic Path Calculation.

pathPara.setAttribute(
  'd',
  `M ${START_X} ${GROUND_Y} Q ${START_X + paraPx/2} ${GROUND_Y - (apexPx * 1.2)} ${START_X + paraPx} ${GROUND_Y}`
);
Real Flight (Bezier curve adjusting for Magnus effect):JavaScriptpathMagnus.setAttribute(
  'd',
  `M ${START_X} ${GROUND_Y} C ${cp1X} ${cp1Y}, ${cp2X} ${cp2Y}, ${endX} ${GROUND_Y}`
);



🔁 Animation Loop & InteractivityThe system utilizes a continuous animation loop to keep the physics updating, rendering smooth, and the UI responsive.
function animate() {
    requestAnimationFrame(animate);
    updatePhysics();
    controls.update();
    renderer.render(scene, camera);
}
User inputs actively alter the physics state and trigger the swing mechanics:

document.getElementById('impactBtn').addEventListener('click', () => {
    simData.mph = parseFloat(document.getElementById('impactSpeedMph').value);
    simState = 'swinging';
});




🧠 Engineering TakeawaysParticle Systems: Can effectively approximate complex material deformations without the overhead of soft-body mesh physics. Real-time physics requires continuous mathematical integration (frame-by-frame updates).Synchronization: Visual models must stay strictly synchronized with mathematical models to maintain illusion and accuracy.Precision: Small corrections (like the landing snap) are critical for overriding floating-point errors.

⚠️ Limitations & 🔮 Future WorkAreaCurrent LimitationPlanned Future WorkAerodynamicsSimplified Magnus effect; no wind or spin decay.Implement real aerodynamic modeling, spin rate (RPM) variables, and wind simulation.PhysicsApproximate material physics; no full fluid dynamics.Integrate GPU-accelerated particle optimization for denser, more accurate deformation calculations.


🛠️ Tech StackFrontend: 
HTML 
CSS3 (Tailwind)Logic 
Vanilla JavaScript3D Rendering 
Three.js 2D Visualization 
Scalable Vector Graphics (SVG)
