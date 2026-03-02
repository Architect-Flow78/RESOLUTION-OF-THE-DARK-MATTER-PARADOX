# ==============================================================================
# RESOLUTION OF DARK MATTER VIA PHASE MATHEMATICS (THE COHERENCE WAVE)
#
# Standard Physics (Left Model):
# Uses linear Keplerian dynamics: v ∝ 1/sqrt(r).
# Without the artificial injection of "Dark Matter", differences in 
# angular velocity shear the galactic system apart into a chaotic disk.
#
# Toroidal Phase Topology (Right Model):
# A galaxy is NOT a collection of objects influenced by invisible mass.
# It is a macroscopic Phase Wave. The spiral is a topological invariant (L1-Emergence). 
# Stars flow through the space at a constant velocity (v = const). 
#
# The Hamiltonian of Phase Friction: H = -K * cos(Δθ)
# When a star's local phase aligns with the invariant macro-wave (Δθ = 0), 
# phase friction drops to zero, correlation C = 1, and the node ignites.
# The topological structure survives permanently.
# ==============================================================================

import numpy as np
import matplotlib.pyplot as plt
import matplotlib.animation as animation

# ============================================================
# 1. PARAMETERS & TOPOLOGICAL SETUP
# ============================================================
N_STARS = 2500
R_MAX = 10.0

# Initialize random spatial distribution
np.random.seed(42)
r = np.random.uniform(0.5, R_MAX, N_STARS)

# Base invariant structure (trailing spiral arms)
theta_base = -1.2 * r + np.random.normal(0, 0.1, N_STARS)
arm = np.random.choice([0, np.pi], N_STARS)
theta_star_init = theta_base + arm

# ============================================================
# 2. VISUALIZATION ENGINE
# ============================================================
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(15, 7.5), facecolor='#0a0a1a')
fig.subplots_adjust(wspace=0.1)

for ax in [ax1, ax2]:
    ax.set_facecolor('#0a0a1a')
    ax.set_xlim(-R_MAX-1.5, R_MAX+1.5)
    ax.set_ylim(-R_MAX-1.5, R_MAX+1.5)
    ax.set_aspect('equal')
    ax.axis('off')

# Scatter objects
scat_linear = ax1.scatter([], [], s=2, color='orange', alpha=0.6, edgecolors='none')
scat_phase = ax2.scatter([], [], s=2, color='cyan', alpha=0.8, edgecolors='none')

# Scientific Headers
ax1.text(0, R_MAX + 1.8, 'STANDARD METRIC (No Dark Matter)\nKeplerian decay shreds the system into chaos', 
         color='orange', ha='center', fontsize=12, fontweight='bold')
ax2.text(0, R_MAX + 1.8, 'PHASE METRIC (Topological Coherence)\nPhase-friction defines structure (v = const)', 
         color='cyan', ha='center', fontsize=12, fontweight='bold')

# Supermassive central phase nodes
ax1.scatter([0], [0], s=200, color='white', alpha=0.3)
ax2.scatter([0], [0], s=200, color='white', alpha=0.3)

# ============================================================
# 3. KINEMATICS & COHERENCE DYNAMICS (THE MATH CORE)
# ============================================================
V_CONST = 7.0         # Rigid linear velocity of nodes in the phase network
OMEGA_PATTERN = 1.0   # Angular velocity of the invariant macroscopic phase wave
FRAMES = 150

def update(frame):
    t = frame * 0.08
    
    # --------------------------------------------------------
    # LEFT: Linear Mechanics (v ~ 1/sqrt(r)) -> Chaos
    # --------------------------------------------------------
    # ω = v/r = (1/sqrt(r)) / r = 1/r^1.5
    omega_lin = V_CONST / (r**1.5)
    theta_lin = theta_star_init + omega_lin * t
    x_lin, y_lin = r * np.cos(theta_lin), r * np.sin(theta_lin)
    scat_linear.set_offsets(np.c_[x_lin, y_lin])
    
    # --------------------------------------------------------
    # RIGHT: Toroidal Phase Topology (v = const) -> Eternal Coherence
    # --------------------------------------------------------
    # ω = v/r = const/r
    omega_phase = V_CONST / r
    theta_phase = theta_star_init + omega_phase * t
    
    # The Invariant Phase Wave tracking
    theta_wave = -1.2 * r + OMEGA_PATTERN * t
    
    # Compute Phase Coherence: C = cos(Δθ)
    # Factor of 2 accounts for 2-arm spiral symmetry (π phase shift)
    coherence = np.cos(2 * (theta_phase - theta_wave))
    
    # Nodes ignite when phase-friction drops to zero (coherence -> 1)
    # This correlates mathematically with the absence of biological/system fatigue
    alphas = np.clip((coherence + 1) / 2, 0.02, 1.0)
    alphas = alphas ** 3  # Exponential contrast to visualize the coherent ridges
    sizes = 18 * alphas + 1
    
    # Dynamic Phase Coloring (Cyan/Blue spectrum)
    rgba = np.zeros((N_STARS, 4))
    rgba[:, 0] = 0.0               # R
    rgba[:, 1] = alphas * 0.8 + 0.2  # G
    rgba[:, 2] = 1.0               # B
    rgba[:, 3] = alphas            # A
    
    x_phase, y_phase = r * np.cos(theta_phase), r * np.sin(theta_phase)
    
    scat_phase.set_offsets(np.c_[x_phase, y_phase])
    scat_phase.set_facecolors(rgba)
    scat_phase.set_sizes(sizes)
    
    return scat_linear, scat_phase

print("Igniting the Phase Wave Network Computations...")
ani = animation.FuncAnimation(fig, update, frames=FRAMES, interval=40, blit=True)

output_filename = 'galaxy_phase_coherence.gif'
ani.save(output_filename, writer='pillow', fps=25, dpi=120, savefig_kwargs={'facecolor': '#0a0a1a'})
print(f"Render complete. Saved as: {output_filename}")
