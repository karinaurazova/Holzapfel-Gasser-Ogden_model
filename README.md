# Holzapfel-Gasser-Ogden_model

## Installation

To run the scripts, you will need to have Python installed on your machine. You can download Python from the official website: https://www.python.org/

You will also need to install the following Python libraries:
```
pip install numpy
pip install matplotlib
```
## Usage

1. Copy the repository to your local machine:
```
git clone https://github.com/karinaurazova/Holzapfel-Gasser-Ogden_model.git
```
3. Run the Python script for the desired model:
```
Holzapfel_Gasser_Ogden_model_unaxial_tension.ipynb: This script presents the implementation of a model for unaxial tension and the prediction of the stress-strain relationship
Holzapfel_Gasser_Ogden_model_biaxial_tension.ipynb: This script presents the implementation of a model for biaxial tension and the prediction of the stress-strain relationship.
```
## Theory
Implementation of the anisotropic Holzapfel-Gasser-Ogden material model for uniaxial and biaxial tension on Python.

Model Holzapfel-Gasser-Ogden (HGO) it is a widely used model to describe the mechanical behavior of anisotropic materials such as biological tissues (e.g. arteries, muscles, skin). This model takes into account both isotropic and anisotropic material properties, which makes it especially useful for modeling tissues with pronounced structural anisotropy, for example, due to the presence of collagen fibers.

The HGO model divides the strain energy into two parts:
1. **Isotropic part** - describes the behavior of the main matrix of the material (for example, elastin in arteries).
2. **Anisotropic part** - takes into account the influence of collagen fibers, which give the material anisotropic properties.

The strain energy $\Psi$ in the HGO model is written as:
```math
\Psi = \Psi_{\text{iso}} + \Psi_{\text{aniso}},
```
where
- $\Psi_{\text{iso}}$ — the isotropic part, often described by the Ogden or Neo-Hookean model.
- $\Psi_{\text{aniso}}$ — anisotropic part, which takes into account the orientation and mechanical behavior of collagen fibers.

The anisotropic part of the strain energy depends on the strain invariants associated with the direction of the fibers:
```math
\Psi_{\text{aniso}} = \sum_{i=1}^{N} \frac{k_1}{2k_2} \left( e^{k_2 (I_{4i} - 1)^2} - 1 \right),
```
where
- $k_1$ и $k_2$ — material parameters that determine the stiffness and nonlinearity of fibers,
- $I_{4i}$ — deformation invariant associated with the i direction of the fiber family.

### Stress calculation
Stresses are calculated as the derivative of strain energy with respect to strain. For this purpose, the second Piola-Kirchhoff stress tensor $\mathbf{S}$ is used:
```math
\mathbf{S} = 2 \frac{\partial \Psi}{\partial \mathbf{C}},
```
where 
$\mathbf{C}$ — right Cauchy-Green deformation tensor.

### Unaxial and biaxial tension
1.Unaxial tension:
   - In this case, the material is stretched in one direction, and deformation in other directions is limited.
   - The HGO model allows us to take into account how collagen fibers oriented in the direction of tension affect the mechanical response of the material.
   - This is useful, for example, for modeling the behavior of arteries when stretched along their axis.

2. Biaxial tension:
   - Here the material is stretched in two directions at the same time.
   - The HGO model takes into account the interaction between fibers oriented in different directions, which is important for describing the complex behavior of tissues such as skin or organ walls.
   - For example, when modeling arteries, biaxial stretching allows us to take into account the influence of both circular and longitudinal collagen fibers.

### What is the HGO model for?
- **Biological tissue modeling**: The HGO model is widely used in biomechanics to describe the behavior of arteries, muscles, skin and other tissues where anisotropy is important.
- **Mechanical response prediction**: It allows you to predict how tissue will deform under external loads, which is important for medical applications such as designing stents or simulating surgical procedures.
- **Damage survey**: The model can be extended to account for tissue damage, such as rupture of collagen fibers.

## HGO for unaxial tension
For uniaxial tension:
- The deformation is specified in one direction (for example, along the x axis).
- Strain invariants $I_1$ and $I_{4i}$ are expressed through the main stretches $\lambda_1$, $\lambda_2$, $\lambda_3$.

For example, if stretching occurs along the x-axis, then:
```math
\lambda_1 = \lambda, \quad \lambda_2 = \lambda_3 = \frac{1}{\sqrt{\lambda}},
```
where
$\lambda$ — stretchin the direction of stretching.

Invariants:
```math
I_1 = \lambda_1^2 + \lambda_2^2 + \lambda_3^2,

I_{4i} = \lambda_1^2 \cos^2 \theta_i + \lambda_2^2 \sin^2 \theta_i,
```
where
$\theta_i$ — angle of orientation of the i-th fiber family relative to the tensile axis.

Stress $\sigma$ in the stretching direction:
```math
\sigma = \frac{\partial \Psi}{\partial \lambda}.
```

In the script, the $\sigma$ stress is calculated as:
```math
\sigma = \frac{\partial \Psi_{\text{total}}}{\partial \epsilon},
```
where
```math
\Psi_{\text{total}} = \Psi_{\text{iso}} + \Psi_{\text{aniso}}.
```

**Derivative of the isotropic part**:
```math
   \begin{equation*}
   \frac{\partial \Psi_{\text{iso}}}{\partial \epsilon} = \frac{\partial}{\partial \epsilon} \left( C (I_1 - 3) \right) = 2C,
   \end{equation*}
```
because
$I_1 = 1 + 2\epsilon$.

**Derivative of the anisotropic part**:
```math
   \begin{equation*}
   \frac{\partial \Psi_{\text{aniso}}}{\partial \epsilon} = \frac{\partial}{\partial \epsilon} \left( \frac{k_1}{2k_2} \left( e^{k_2 E_{\text{fiber}}^2} - 1 \right) \right),
   \end{equation*}
```
where
```math
   \begin{equation*}
   E_{\text{fiber}} = \kappa (I_1 - 3) + (1 - 3\kappa)(I_4 - 1).
   \end{equation*}
```
   **Derivative of $E_{\text{fiber}}$ with respect to $\epsilon$**:
```math
   \begin{equation*}
   \frac{\partial E_{\text{fiber}}}{\partial \epsilon} = 2\kappa + 2(1 - 3\kappa) \cos^2(\theta).
   \end{equation*}
```
Then:
```math
   \begin{equation*}
   \frac{\partial \Psi_{\text{aniso}}}{\partial \epsilon} = k_1 E_{\text{fiber}} e^{k_2 E_{\text{fiber}}^2} \frac{\partial E_{\text{fiber}}}{\partial \epsilon}.
   \end{equation*}
```

**Stress**:
```math
   \begin{equation*}
   \sigma = \frac{\partial \Psi_{\text{iso}}}{\partial \epsilon} + \frac{\partial \Psi_{\text{aniso}}}{\partial \epsilon}.
   \end{equation*}
```

Example of the resulting stress-strain curves for uniaxial tension:
![](https://github.com/karinaurazova/Holzapfel-Gasser-Ogden_model/blob/main/H-G-O(unaxial%20tension).png)
---

## HGO for biaxial tension
For biaxial tension:
- Deformation is specified in two directions (for example, along the x and y axes).
- Main stretches:
```math
  \lambda_1 = \lambda_x, \quad \lambda_2 = \lambda_y, \quad \lambda_3 = \frac{1}{\lambda_x \lambda_y}.
```
Invariants:
```math
I_1 = \lambda_x^2 + \lambda_y^2 + \lambda_3^2,

I_{4i} = \lambda_x^2 \cos^2 \theta_i + \lambda_y^2 \sin^2 \theta_i.
```
Stresses $\sigma_x$ and $\sigma_y$ in tensile directions:
```math
\sigma_x = \frac{\partial \Psi}{\partial \lambda_x}, \quad \sigma_y = \frac{\partial \Psi}{\partial \lambda_y}.
```

**Derivative of strain energy**:
   - For the isotropic part $\Psi_{\text{iso}} = C (I_1 - 3)$:
```math
     \frac{\partial \Psi_{\text{iso}}}{\partial \epsilon_x} = 2C, \quad \frac{\partial \Psi_{\text{iso}}}{\partial \epsilon_y} = 2C.
```
   - For the anisotropic part $Psi_{\text{aniso}}$:
```math
     \frac{\partial \Psi_{\text{aniso}}}{\partial \epsilon_x} = k_1 E_{\text{fiber}_x} e^{(k_2 E_{\text{fiber}_x}^2)} \frac{\partial E_{\text{fiber}_x}}{\partial \epsilon_x},
```
where
$E_{\text{fiber}_x} = \kappa (I_1 - 3) + (1 - 3\kappa)(I_4 - 1)$.

**Stress**:
   - Strss in x direction:
```math
     \sigma_x = \frac{\partial \Psi_{\text{iso}}}{\partial \epsilon_x} + \frac{\partial \Psi_{\text{aniso}_x}}{\partial \epsilon_x}.
```
   - Stress in y direction:
```math
     \sigma_y = \frac{\partial \Psi_{\text{iso}}}{\partial \epsilon_y} + \frac{\partial \Psi_{\text{aniso}_y}}{\partial \epsilon_y}.
```

Example of the resulting stress-strain curves for biaxial tension:
![](https://github.com/karinaurazova/Holzapfel-Gasser-Ogden_model/blob/main/h-g-o(biaxial%20tension).png)
## Contributing

If you have any suggestions or improvements for the scripts, feel free to open a pull request (or you can email me: karina_urazova@icloud.com) or create an issue on the repository.

***
**Thank you for your interest in the Holzapfel-Gasser-Ogden model anisotropic material for uniaxial and biaxial tensionproject. I hope this framework proves valuable for your research and applications.**


