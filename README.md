# Robotic-Arm-Kinematics-Analysis
Deep analytical study of robotic arm kinematics, featuring derivation of forward kinematics equations, coordinate space mapping, and implementation for multi-DOF industrial manipulators.

# 🤖 Robotic Arm Kinematics Analysis

This repository provides an in-depth analytical study of the Forward Kinematics for a 4-DOF robotic arm, serving as a fundamental reference for robotic motion control.

---

## 📚 1. Introduction to Robotic Kinematics

Kinematics is the study of motion without regard to the forces that cause it. In robotics, it is the mathematical bridge between **Joint Space** (motor angles) and **Task Space** (the XYZ position of the end-effector).

### 1.1 Types of Kinematics
*   **Forward Kinematics (FK):** The "geometry-based" approach. Given the set of joint angles, we calculate the end-effector pose. It is a direct calculation using trigonometric chains.
*   **Inverse Kinematics (IK):** The "target-based" approach. Given a desired XYZ coordinate, we solve for the joint angles. This is mathematically complex, often involving multiple solutions (elbow-up vs. elbow-down configurations) and requiring numerical or iterative methods.

### 1.2 The Kinematic Chain
A robotic arm is essentially a series of **Rigid Bodies** (Links) connected by **Joints**. 
*   **Links:** The physical structural elements.
*   **Joints:** The actuators that provide Degrees of Freedom (DOF).
*   **Kinematic Chain:** The mathematical representation where the pose of one link is calculated relative to the previous link using **Transformation Matrices**.

---

## 2. Practical Analysis: 4-DOF Robotic Arm

<img width="373" height="235" alt="Screenshot 2026-08-13 013208" src="https://github.com/user-attachments/assets/ef7f1b54-1601-4f49-ac2c-b14e9ade8a57" />

The arm under analysis is a serial manipulator with 4 independent joints. The goal is to derive the position of the end-effector by mapping the angular displacement ($\theta$) of each motor.

### 2.1 Geometric Parameters
| Joint | Description | Length (mm) | Variable |
| :--- | :--- | :--- | :--- |
| Joint 1 | Base Rotation | - | $\theta_1$ |
| Joint 2 | Shoulder | 100 | $\theta_2$ |
| Joint 3 | Elbow | 65 | $\theta_3$ |
| Joint 4 | Wrist | 15 | $\theta_4$ |

### 2.2 Mathematical Derivation
We model the arm using a planar representation in the $(r, Z)$ plane, where $r$ is the radial distance from the base axis.

**Step 1: Projecting the link segments**
Each link contributes to both the radial distance ($r$) and the vertical height ($Z$).
*   Link 1 ($L_1=100$) at angle $\theta_2$
*   Link 2 ($L_2=65$) at combined angle $\theta_2 + \theta_3$
*   Link 3 ($L_3=15$) at combined angle $\theta_2 + \theta_3 + \theta_4$

**Step 2: Defining the Radial distance ($r$)**
The radial reach is the sum of the horizontal projections:
$$r = L_1 \cos(\theta_2) + L_2 \cos(\theta_2 + \theta_3) + L_3 \cos(\theta_2 + \theta_3 + \theta_4)$$

**Step 3: Defining the Vertical height ($Z$)**
The vertical reach is the sum of the vertical projections:
$$Z = L_1 \sin(\theta_2) + L_2 \sin(\theta_2 + \theta_3) + L_3 \sin(\theta_2 + \theta_3 + \theta_4)$$

**Step 4: Mapping to 3D Cartesian Space**
To project the planar distance $r$ into the 3D space $(X, Y)$ relative to the base rotation $\theta_1$:
*   $X = r \cdot \cos(\theta_1)$
*   $Y = r \cdot \sin(\theta_1)$

---

## 3. Final Implemented Kinematic Equations

By substituting $r$ into the Cartesian mapping equations, we obtain the final state-space model used for the robot's control firmware:

### Global Coordinates:
1.  **$X = [100\cos(\theta_2) + 65\cos(\theta_2+\theta_3) + 15\cos(\theta_2+\theta_3+\theta_4)] \cdot \cos(\theta_1)$**
2.  **$Y = [100\cos(\theta_2) + 65\cos(\theta_2+\theta_3) + 15\cos(\theta_2+\theta_3+\theta_4)] \cdot \sin(\theta_1)$**
3.  **$Z = 100\sin(\theta_2) + 65\sin(\theta_2+\theta_3) + 15\sin(\theta_2+\theta_3+\theta_4)$**

### Control Implications:
These equations allow the control system to translate any high-level movement command into precise motor control inputs. By calculating the Jacobian matrix of these equations in the future, we can further implement Velocity Kinematics and force distribution control.

---
*Technical Documentation - Robotics Mechanics Track | Smart Methods*
