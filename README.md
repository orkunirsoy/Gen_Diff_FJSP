# Generative Diffusion Models for Flexible Job Shop Scheduling

This repository contains our ongoing research project on using discrete diffusion models to solve the Flexible Job Shop Scheduling Problem (FJSP).

In FJSP, each job consists of a sequence of operations, and each operation can be processed by one of several eligible machines. The goal is to assign operations to machines and determine their processing order while minimizing the total completion time (makespan).

## Approach

We use a heterogeneous graph neural network within a discrete diffusion framework to jointly generate machine assignments and operation priorities. The generated decisions are converted into feasible schedules using a deterministic decoder that respects job precedence and machine availability constraints.

The model is trained on reference schedules obtained from Google OR-Tools CP-SAT. We evaluate it on synthetic and standard benchmark instances, comparing its performance against dispatching heuristics, evolutionary algorithms, and reinforcement learning methods.

## Current Progress

We have implemented the diffusion model, schedule decoders, and evaluation pipeline. Preliminary experiments show that non-delay decoding significantly improves schedule quality compared to our initial priority-first decoder. Generating multiple candidate schedules and selecting the best also reduces the makespan gap.

The model is competitive with some evolutionary baselines when using multiple samples, but there is still room for improvement compared to stronger learning-based methods.

![Makespan gap comparison](figures/all_methods_gap_boxplots.pdf)

*Figure 1. Makespan gaps relative to CP-SAT across five test sets. Lower is better.*

## Next Steps

We plan to explore several improvements to the current approach:

- **Distance-aware priority loss:** Account for the distance between predicted and target priority values during training.
- **Multiple reference schedules:** Train using several optimal or near-optimal schedules per instance rather than a single reference solution.
- **Objective-guided sampling:** Guide the generation process toward schedules with lower makespan.
- **Schedule repair:** Investigate diffusion inpainting to update part of an existing schedule when machine availability or processing times change.
- **Discrete flow matching:** Explore alternative generative formulations that may require fewer sampling steps.

The immediate focus is on improving the training objective and schedule generation quality before extending the model to more flexible scheduling settings.

## Repository Structure

- `train/`: Diffusion model and training code
- `scripts/`: Data generation, scheduling, and evaluation scripts
- `data/`: Synthetic and benchmark FJSP instances
- `CP-SAT/`: Reference solutions from the CP-SAT solver
- `baselines/`: Heuristic, evolutionary, and learning-based baselines
- `runs/`: Model checkpoints and experiment outputs
- `figures/`: Experimental plots and visualizations
- `tests/`: Validation and testing code

## Status

This is an ongoing research project. The implementation and experimental results may change as we continue developing and evaluating the model.
