# Nonlinear Mathematical Modeling of Succinate–Immune–Stromal Signaling in Crohn’s Disease

### A mechanistic ODE framework for fibrostenotic remodeling

This repository contains the computational implementation and numerical analyses accompanying the research project:

**“Nonlinear Mathematical Modeling of Succinate–Immune–Stromal Signaling in Crohn’s Disease: A Mechanistic ODE Framework for Fibrostenotic Remodeling.”**

The project develops a nonlinear, seven-state ordinary differential equation (ODE) model to investigate how inflammatory activity, succinate-associated signaling, inflammatory monocytes, TWIST1-associated activity, FAP-positive fibroblasts, and extracellular matrix (ECM) accumulation may interact during intestinal fibrostenotic remodeling in Crohn’s disease.

The model brings together two proposed signaling routes that converge at the fibroblast compartment:

1. A succinate–SUCNR1-associated signaling route.
2. A CD150-positive inflammatory monocyte–TWIST1-associated route.

The purpose is to provide a transparent mathematical framework for exploring the dynamics, feedback mechanisms, equilibrium behavior, parameter sensitivity, and uncertainty of these interacting processes.

This is a computational and hypothesis-generating modeling study. The model is not intended to represent a clinically validated prediction tool or a patient-specific estimate of intestinal fibrosis.

---

## 1. Research background and motivation

Intestinal fibrosis is an important complication of Crohn’s disease and is associated with chronic inflammation, altered stromal-cell activity, extracellular matrix deposition, and progressive tissue remodeling.

Several biological studies have highlighted the potential roles of succinate-associated inflammatory signaling and interactions between inflammatory monocytes and fibroblast populations in intestinal inflammation and fibrosis.

However, the dynamic relationships among these processes are not necessarily captured by considering each biological component independently.

This project therefore uses a mechanistic ODE framework to explore how two upstream signaling routes may contribute to a shared fibroblast-mediated ECM remodeling process.

The model is designed around a specific mathematical question:

**How can interacting inflammatory, metabolic, immune-cell, and stromal signaling processes generate different transient and long-term ECM responses under a set of explicitly defined mechanistic assumptions?**

The emphasis is on mathematical structure, dynamical behavior, and computational reproducibility rather than on claiming direct experimental causality for every modeled interaction.

---

## 2. Model structure

The model consists of seven state variables:

| Variable | Biological interpretation |
|---|---|
| \(I(t)\) | Inflammatory activity |
| \(S(t)\) | Extracellular succinate |
| \(R(t)\) | SUCNR1-associated signaling activity |
| \(M(t)\) | CD150-positive inflammatory monocyte activity |
| \(T(t)\) | TWIST1-associated activity |
| \(F(t)\) | FAP-positive fibroblast activity |
| \(C(t)\) | Extracellular matrix (ECM) burden |

The state vector is

\[
X(t)=[I(t),S(t),R(t),M(t),T(t),F(t),C(t)]^T.
\]

Two signaling routes are represented:

**Route A: Succinate-associated signaling**

\[
I \rightarrow S \rightarrow R \rightarrow F \rightarrow C
\]

**Route B: Monocyte-associated signaling**

\[
I \rightarrow M \rightarrow T \rightarrow F \rightarrow C
\]

Both routes contribute additively to fibroblast activation in the model.

The convergence at \(F\) represents a modeling hypothesis about combined upstream contributions. It does not assume a direct molecular interaction between SUCNR1-associated signaling and TWIST1.

### Model diagram

The seven-state model structure is provided in:

`figures/seven_state_model_structure.png`

---

## 3. Mathematical formulation

The system is defined by the following nonlinear ODEs:

\[
\frac{dI}{dt}
=
u(t)+\beta_RR+\beta_MM-\delta_II
\]

\[
\frac{dS}{dt}
=
p_SI-\delta_SS
\]

\[
\frac{dR}{dt}
=
\alpha_R\frac{S}{K_S+S}
-\delta_RR
\]

\[
\frac{dM}{dt}
=
\alpha_M\frac{I}{K_I+I}
-\delta_MM
\]

\[
\frac{dT}{dt}
=
\alpha_T\frac{M}{K_M+M}
-\delta_TT
\]

\[
\frac{dF}{dt}
=
\alpha_{FR}\frac{R}{K_{FR}+R}
+
\alpha_{FT}\frac{T}{K_{FT}+T}
-\delta_FF
\]

\[
\frac{dC}{dt}
=
p_CF-\delta_CC.
\]

The saturating activation function is represented by

\[
H(x;K)=\frac{x}{K+x}.
\]

Here, \(u(t)\) denotes an external inflammatory input. The saturating functions represent bounded activation responses, while the removal terms describe effective first-order loss processes.

All state variables are treated as normalized effective activities rather than absolute concentrations, cell counts, or directly measured clinical quantities.

---

## 4. Baseline parameterization

The baseline parameter values used in the computational experiments are:

| Parameter | Baseline value |
|---|---:|
| \(\beta_R\) | 0.03 |
| \(\beta_M\) | 0.03 |
| \(\delta_I\) | 0.50 |
| \(p_S\) | 0.40 |
| \(\delta_S\) | 1.00 |
| \(\alpha_R\) | 0.60 |
| \(K_S\) | 0.50 |
| \(\delta_R\) | 0.70 |
| \(\alpha_M\) | 0.50 |
| \(K_I\) | 0.50 |
| \(\delta_M\) | 0.35 |
| \(\alpha_T\) | 0.45 |
| \(K_M\) | 0.50 |
| \(\delta_T\) | 0.25 |
| \(\alpha_{FR}\) | 0.35 |
| \(K_{FR}\) | 0.50 |
| \(\alpha_{FT}\) | 0.45 |
| \(K_{FT}\) | 0.50 |
| \(\delta_F\) | 0.12 |
| \(p_C\) | 0.25 |
| \(\delta_C\) | 0.025 |

These values are exploratory working assumptions used to investigate the mathematical behavior of the proposed system. They should not be interpreted as experimentally estimated biological constants or clinically fitted parameters.

The complete parameter definitions and sampling ranges are maintained in the Jupyter notebook.

---

## 5. Numerical experiments and computational analyses

The computational workflow is organized in `01_model_setup.ipynb`.

It includes the following analyses.

### 5.1 Baseline transient simulation

A transient inflammatory input is used to examine the temporal response of the seven-state system.

The baseline experiment uses an inflammatory pulse with amplitude 0.2 over the first 2 days, followed by a simulation over a 60-day period.

The resulting trajectories are used to examine the sequential response of inflammatory, metabolic, immune, stromal, and ECM-related state variables.

### 5.2 Equilibrium analysis

The equilibrium structure is investigated by solving the steady-state conditions of the model.

The analysis considers the disease-free equilibrium and possible positive equilibria under the specified parameter configurations.

A model-specific feedback threshold is also evaluated to characterize the local behavior of the inflammatory subsystem.

This threshold is a mathematical quantity derived from the proposed model and should not be interpreted as an epidemiological reproduction number.

### 5.3 Local stability analysis

The Jacobian matrix is evaluated at relevant equilibria.

Eigenvalue analysis is used to investigate local asymptotic stability under the model assumptions.

### 5.4 One-at-a-time sensitivity analysis

A local one-at-a-time (OAT) sensitivity analysis is performed by perturbing individual parameters around their baseline values.

The analysis examines the effects of parameter changes on:

- Peak ECM burden.
- Final ECM burden.
- ECM area under the curve (AUC).
- Time to peak ECM.

The baseline perturbation magnitude is ±10%.

These results describe local parameter sensitivity and are not intended to represent a global sensitivity ranking.

### 5.5 Latin hypercube sampling and uncertainty analysis

Latin hypercube sampling (LHS) is used to explore parameter uncertainty across the specified parameter ranges.

The computational design uses 200 parameter samples and a fixed random seed of `20261110`.

For each sampled parameter set, the model is numerically integrated and the resulting ECM-related outcomes are recorded.

The uncertainty analysis summarizes the distribution of:

- Peak ECM.
- Final ECM.
- ECM AUC.
- Time to peak ECM.

The reported percentiles represent conditional simulation summaries under the selected sampling ranges. They are not clinical confidence intervals.

### 5.6 Parameter–outcome association

Spearman rank correlation is used to examine monotonic associations between sampled parameters and selected model outcomes.

These correlations provide an additional exploratory view of the relationship between parameter variation and simulated system behavior.

### 5.7 Long-term equilibrium diagnostics

Extended numerical simulations are used to investigate whether selected transient responses approach their corresponding equilibrium states.

A longer simulation horizon is used for diagnostic purposes. Peaks occurring at the final simulation time are treated as horizon-limited observations rather than evidence of permanent or irreversible fibrosis.

---

## 6. Repository organization

The repository is organized as follows:

```text
crohns-fibrostenosis-ode-model/
│
├── 01_model_setup.ipynb
│
├── figures/
│   ├── seven_state_model_structure.png
│   ├── model_A_transient_pulse.png
│   ├── sensitivity_peak_ecm.png
│   ├── sensitivity_final_ecm.png
│   ├── sensitivity_ecm_auc.png
│   ├── sensitivity_time_to_peak_days.png
│   ├── uncertainty_peak_ecm.png
│   ├── uncertainty_final_ecm.png
│   ├── uncertainty_ecm_auc.png
│   ├── uncertainty_time_to_peak_days.png
│   └── ...
│
├── results/
│   └── Numerical result tables and computational summaries
│
├── LICENSE
│
└── README.md
```

The notebook contains the model implementation, parameter definitions, numerical simulations, analysis procedures, and figure-generation routines.

The `figures/` directory contains the generated graphical outputs in publication-friendly formats, including PNG and PDF files. The model diagram is also available in SVG format.

The `results/` directory contains the numerical outputs and supporting computational summaries generated by the notebook.

---

## 7. Software and reproducibility

The computational workflow is implemented in Python using commonly available scientific-computing libraries.

The main dependencies are:

- Python 3
- NumPy
- SciPy
- pandas
- Matplotlib
- Jupyter Notebook or JupyterLab

A compatible environment can be prepared using:

```bash
pip install numpy pandas scipy matplotlib jupyterlab
```

### Running the notebook

1. Clone or download this repository.
2. Open the repository directory in JupyterLab, Jupyter Notebook, or a compatible VS Code environment.
3. Open `01_model_setup.ipynb`.
4. Install the required dependencies if they are not already available.
5. Execute the notebook sequentially from the first cell to the final validation and export section.

The notebook creates the required output directories and saves the generated figures and numerical results during execution.

For reproducibility, the LHS random seed is fixed at `20261110`.

The numerical outputs may vary slightly across software environments or solver configurations due to floating-point and numerical integration differences.

---

## 8. Interpretation and scope

The model is intended to support mechanistic exploration of interacting inflammatory, metabolic, immune, and stromal processes in Crohn’s disease.

Several aspects of the framework require careful interpretation:

- State variables represent normalized effective activities, not directly measured biological quantities.
- Parameter values are exploratory assumptions rather than estimates obtained from patient-level data.
- The additive convergence of the two signaling routes at the fibroblast compartment is a modeling hypothesis.
- The ECM state represents an effective matrix burden and is not equivalent to a validated histological or clinical fibrosis score.
- The model does not explicitly represent spatial heterogeneity, collagen cross-linking, matrix metalloproteinase activity, tissue mechanics, or all known inflammatory and stromal cell populations.
- The framework does not establish clinical predictive performance or therapeutic efficacy.

The results should therefore be interpreted as mathematical and computational findings conditional on the model structure, parameterization, and numerical experimental design.

---

## 9. Biological references

The model development is informed by published research on succinate signaling, intestinal inflammation, inflammatory monocytes, fibroblast populations, and intestinal fibrosis.

Selected references include:

1. Macias-Ceja DC, et al. Succinate receptor mediates intestinal inflammation and fibrosis. *Mucosal Immunology*. 2019;12:178–187.  
   https://doi.org/10.1038/s41385-018-0087-3

2. Ke B-J, et al. Intercellular interaction between FAP+ fibroblasts and CD150+ inflammatory monocytes mediates fibrostenosis in Crohn’s disease. *Journal of Clinical Investigation*. 2024;134(16):e173835.  
   https://doi.org/10.1172/JCI173835

3. Zhang Y, et al. TWIST1+FAP+ fibroblasts in the pathogenesis of intestinal fibrosis in Crohn’s disease. *Journal of Clinical Investigation*. 2024;134(18):e179472.  
   https://doi.org/10.1172/JCI179472

4. Mukherjee PK, et al. Stricturing Crohn’s disease single-cell RNA sequencing reveals fibroblast heterogeneity and intercellular interactions. *Gastroenterology*. 2023;165(5):1180–1196.  
   https://doi.org/10.1053/j.gastro.2023.07.014

5. Rieder F, et al. Fibrosis in IBD: from pathogenesis to therapeutic targets. *Gut*. 2024;73(5):854–866.  
   https://doi.org/10.1136/gutjnl-2023-329963

6. Tannahill GM, et al. Succinate is an inflammatory signal that induces IL-1β through HIF-1α. *Nature*. 2013;496:238–242.  
   https://doi.org/10.1038/nature11986

These references provide biological context for the modeled pathways. Their inclusion does not imply that every mathematical interaction or parameter value has been experimentally established.

---

## 10. Citation and use

If you use this repository, its model structure, or its computational implementation in academic work, please cite the associated research manuscript when a citable publication or preprint becomes available.

For work that directly reuses the code, please also refer to this repository and its version or commit identifier.

The repository is intended to support transparency, inspection, and reproducibility of the computational framework.

---

## 11. Author and research affiliation

**Rifaldy Fajar**  
WorldQuant University, USA  
Email: rifaldyfajar251@gmail.com

The affiliation is provided for author identification only. It should not be interpreted as an institutional endorsement, supervision, funding relationship, or official representation of the research project by WorldQuant University.

---

## 12. License

This project is distributed under the MIT License. See the `LICENSE` file for the full license terms.

---

*This repository is maintained as a computational research resource accompanying the development of a nonlinear mathematical framework for investigating inflammatory and stromal signaling in Crohn’s disease-associated fibrostenotic remodeling.*
