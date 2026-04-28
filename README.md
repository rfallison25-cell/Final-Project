🧪 Atomic Golf Impact Lab

A real-time 3D + physics simulation of golf ball impact and flight

This project simulates the full lifecycle of a golf shot:

Impact deformation (particle-based)
Energy transfer through the ball
Projectile motion with real physics constraints
Dynamic trajectory visualization
🚀 Quickstart
git clone https://github.com/yourusername/atomic-golf-impact-lab.git
cd atomic-golf-impact-lab
open index.html
⚙️ Core Architecture

The system is split into two synchronized engines:

3D Physics Engine (Three.js)
2D Trajectory Engine (SVG + math)

Both are driven by the same inputs:

let mph = parseFloat(document.getElementById('impactSpeedMph').value) || 100;
let clubKey = document.getElementById('clubSelect').value;
let clubParams = CLUBS[clubKey];
🧱 Particle-Based Golf Ball

Instead of a single mesh, the ball is composed of thousands of particles:

const ATOM_COUNT = 3500;
const BALL_RADIUS = 4.0;

Each particle stores its original position:

atom.userData = { originalPos: atom.position.clone() };

Particles are distributed in a spherical volume:

const phi = Math.acos(-1 + (2 * i) / ATOM_COUNT);
const theta = Math.sqrt(ATOM_COUNT * Math.PI) * phi;

This creates a dense, realistic structure for deformation.

💥 Impact & Deformation Logic

During impact, each particle is checked against the club face:

let worldX = ballGroup.position.x + atom.userData.originalPos.x;
let clubFaceX = simData.clubX + 0.8;

if (worldX < clubFaceX) {
    let push = clubFaceX - worldX;
    targetPos.x += push;
}

This produces:

Compression
Flattening against the club
Directional force response
🌊 Shockwave Propagation

Energy travels through the ball using a radial wave:

simData.waveRadius += 14.0 * dt;

let distToEpicenter = atom.userData.originalPos.distanceTo(epicenter);
let diffFromWave = Math.abs(distToEpicenter - simData.waveRadius);

Force is applied based on distance from the wave:

let intensity = 1.0 - (diffFromWave / waveWidth);
let force = intensity * decay * (simData.mph / 20);
🎨 Stress Visualization

Stress is mapped to color using interpolation:

targetColor.setHSL(
  0.6 - (Math.min(stressLevel, 1) * 0.6),
  0.9,
  0.5
);
Blue = low stress
Red = high stress
🚀 Launch Physics

The simulation ensures the ball lands at the correct distance by solving for velocity:

v=
sin(2θ)
D⋅g
	​

	​


Implemented in code:

const launchVelocity = Math.sqrt(
  (targetCarry * GRAVITY) / Math.sin(LOFT_RAD * 2)
);
🛰️ Flight Simulation

Once airborne, motion is updated per frame:

ballGroup.position.x += simData.ballVel.x * dt;
ballGroup.position.y += simData.ballVel.y * dt;

Gravity is applied continuously:

simData.ballVel.y -= GRAVITY * dt;
🎯 Landing Correction

To eliminate floating-point drift, final position is snapped:

let exactCarry = simData.mph * clubParams.multiplier;
ballGroup.position.x = exactCarry;

This ensures:

Perfect accuracy
Match with 2D graph
📈 Trajectory Graph (SVG)

The trajectory is generated dynamically:

Parabolic path:
pathPara.setAttribute(
  'd',
  `M ${START_X} ${GROUND_Y} Q ${START_X + paraPx/2} ${GROUND_Y - (apexPx * 1.2)} ${START_X + paraPx} ${GROUND_Y}`
);
Real flight (Bezier curve):
pathMagnus.setAttribute(
  'd',
  `M ${START_X} ${GROUND_Y} C ${cp1X} ${cp1Y}, ${cp2X} ${cp2Y}, ${endX} ${GROUND_Y}`
);
🔁 Animation Loop

Everything runs inside a continuous loop:

function animate() {
    requestAnimationFrame(animate);
    updatePhysics();
    controls.update();
    renderer.render(scene, camera);
}

This keeps:

Physics updating
Rendering smooth
UI responsive
🎛️ Controls & Interactivity

User inputs directly affect physics:

document.getElementById('impactBtn').addEventListener('click', () => {
    simData.mph = parseFloat(document.getElementById('impactSpeedMph').value);
    simState = 'swinging';
});
🧠 Engineering Takeaways
Particle systems can approximate material deformation
Real-time physics requires continuous integration (frame updates)
Visual models must stay synchronized with mathematical models
Small corrections (like landing snap) are critical for accuracy
⚠️ Limitations
No full fluid dynamics
Simplified Magnus effect
No spin decay or wind
Approximate material physics
🔮 Future Work
Add spin rate (RPM)
Wind simulation
GPU particle optimization
Real aerodynamic modeling
🛠️ Tech Stack
HTML / CSS (Tailwind)
JavaScript
Three.js
SVG rendering
