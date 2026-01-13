# Projectile-Simulator
A not well to do but interested in Physics Guy has made this Simple(Don't mind seeing a low Dumbass code sequence ;)Simulator. Also There are few bugs like the velocity acc. vectors and feq changes would be made into the future





import math
import matplotlib.pyplot as plt

class projectile:
    def __init__(self, angle, velocity, height, accx, accy):
        self.angle = angle
        self.velocity = velocity
        self.height = height
        self.accx = accx
        self.accy = accy
        self.angle_rad = math.radians(self.angle)
        self.ux = velocity * math.cos(self.angle_rad)
        self.uy = velocity * math.sin(self.angle_rad)
        # Correct discriminant and time of flight
        self.discriminant = self.uy**2 - 2*self.accy*self.height
        self.T = (-self.uy - math.sqrt(self.discriminant)) / self.accy if self.discriminant >= 0 else 0
        self.R = self.ux*self.T + 0.5*self.accx*self.T**2
        self.H = self.height + (self.uy**2)/(2*-self.accy)

    def instant(self, time):
        x = self.ux*time + 0.5*self.accx*time**2
        y = self.height + self.uy*time + 0.5*self.accy*time**2
        vx = self.ux + self.accx*time
        vy = self.uy + self.accy*time
        vt = math.sqrt(vx**2 + vy**2)
        fangle = math.degrees(math.atan2(vy, vx))
        return (x, y, vx, vy, vt, fangle)

    def details(self):
        print(f"Time of Flight: {self.T:.2f} s")
        print(f"Range: {self.R:.2f} m")
        print(f"Maximum Height: {self.H:.2f} m")

    def simulate(self, dt=0.01):
        self.details()
        t = 0
        x_list = []
        y_list = []
        while t <= self.T:
            x, y, vx, vy, vt, fangle = self.instant(t)
            x_list.append(x)
            y_list.append(y)
            t += dt
            print(f"t={t:.2f} s: x={x:.2f} m, y={y:.2f} m, vx={vx:.2f} m/s, vy={vy:.2f} m/s, vt={vt:.2f} m/s, angle={fangle:.2f}°")
        return x_list, y_list
# Create projectile
projectile1 = projectile(45, 50, 0, 0, -9.81)

# Simulate and get trajectory points
x_vals, y_vals = projectile1.simulate()

# Plot trajectory with points
plt.figure(figsize=(8,5))
plt.plot(x_vals, y_vals, 'bo-', label="Projectile Path")  # 'bo-' = blue circles + line
# plot velocity vectors at intervals
for t in [i*0.5 for i in range(int(projectile1.T/0.5))]:
     x, y, vx, vy, *_ = projectile1.instant(t)
     plt.quiver(x, y, vx, vy, angles='xy', scale_units='xy', scale=200, color='r')
plt.title("Projectile Motion Trajectory")
plt.xlabel("Horizontal Distance (m)")
plt.ylabel("Vertical Height (m)")
plt.grid(True)
plt.legend()
plt.show()
