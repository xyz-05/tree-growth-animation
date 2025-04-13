# tree-growth-animation
import matplotlib.pyplot as plt
import numpy as np
import matplotlib.animation as animation
import random

# Parameters
max_depth = 7
initial_angle = np.pi / 2  # Straight up
branch_angle_range = (np.pi / 8, np.pi / 4)  # Random branch angles
initial_length = 1
growth_factor = 0.7

# Set up the figure
fig, ax = plt.subplots(figsize=(6, 8))
ax.set_xlim(-2, 2)
ax.set_ylim(0, 3)
ax.axis('off')

# Function to draw branches and leaves
def draw_branch(x, y, angle, length, depth):
    if depth == 0:
        # Draw a leaf with random green color and size
        leaf_color = random.choice(['#228B22', '#32CD32', '#006400'])  # Shades of green
        ax.scatter(x, y, color=leaf_color, s=random.randint(20, 40), zorder=5)
        return

    x_end = x + length * np.cos(angle)
    y_end = y + length * np.sin(angle)

    # Thinner branches at higher depth
    lw = max(1, depth * 0.6)
    ax.plot([x, x_end], [y, y_end], color='saddlebrown', lw=lw, solid_capstyle='round')

    # Recursively draw child branches with slight randomization
    for delta in [-1, 1]:
        new_angle = angle + delta * random.uniform(*branch_angle_range)
        draw_branch(x_end, y_end, new_angle, length * growth_factor, depth - 1)

# Animation function
def animate(frame):
    ax.clear()
    ax.set_xlim(-2, 2)
    ax.set_ylim(0, 3)
    ax.axis('off')

    current_depth = min(frame // 5 + 1, max_depth)
    draw_branch(0, 0, initial_angle, initial_length, current_depth)

# Create animation
ani = animation.FuncAnimation(fig, animate, frames=40, interval=200)
plt.show()
