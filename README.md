# diffusion2d

## Instructions for students

Please follow the instructions in [pypi_exercise.md](https://github.com/Simulation-Software-Engineering/Lecture-Material/blob/main/03_building_and_packaging/pypi_exercise.md).

The code used in this exercise is based on [Chapter 7 of the book "Learning Scientific Programming with Python"](https://scipython.com/book/chapter-7-matplotlib/examples/the-two-dimensional-diffusion-equation/).

## Description

This small Python project solves the two-dimensional diffusion (heat) equation on a square plate using a finite-difference scheme.

Key points about the model and the code:

- The computational domain is a square plate of size `w x h` (units in mm in the example).
- The plate is initialized at a base temperature `T_cold` and contains a circular region in the centre at a higher temperature `T_hot`.
- The time integration is performed with an explicit forward-difference scheme, while spatial derivatives are approximated with central differences.
- The code computes a stable timestep `dt` for the explicit scheme from the grid spacings `dx`, `dy` and the thermal diffusivity `D`.
- Parameters such as `dx`, `dy`, `D` and the number of timesteps can be changed by the user.

Code layout and functions (current repository state):

- `diffusion2d/diffusion2d/diffusion2d.py` – main solver module. It defines `solve(w,h,dx,dy,D,T_cold,T_hot)` and also can be executed as a script (it calls `solve(...)` in the `if __name__ == "__main__"` guard).
- `diffusion2d/diffusion2d/output.py` – plotting helper functions that the solver uses to create and finalize subplots.

Output:

The script produces four snapshots of the temperature distribution at selected timesteps and arranges them in a single figure with a shared colorbar so the diffusion of heat is easy to observe.

## Installing the package

This repository is an exercise project and does not ship as an installable package by default. To run the example you only need Python (>= 3.6) and the required libraries (NumPy and Matplotlib).

Recommended steps (macOS / Linux / WSL):

1. Check Python version:

```bash
python3 --version
```

2. If your Python is older than 3.6, update it (for example via Homebrew on macOS):

```bash
brew install python
```

3. (Optional) Create and activate a virtual environment:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

4. Update pip and install packaging tools (if you want to build/upload a package):

```bash
python3 -m pip install --upgrade pip build twine
```

5. Install dependencies:

```bash
python3 -m pip install numpy matplotlib
```

If you later want to turn this project into a real Python package, follow the steps in `pypi_exercise.md`.

## Running this package

Run the example script from the repository root. Note: in this repository the solver script is placed in the `diffusion2d/` subfolder, so run:

```bash
python3 diffusion2d/diffusion2d/diffusion2d.py
```

Alternatively you can import and call the solver from Python:

```python
from diffusion2d.diffusion2d import solve
# call with default example parameters
solve(10., 10., 0.1, 0.1, 4., 300, 700)
```

What to expect:


- The script prints the computed timestep `dt`.
- A plotting window opens showing four subplots (temperature fields at different times) and a shared colorbar.

Notes about plotting helpers:

- `diffusion2d/diffusion2d/output.py` provides two convenience functions used by the solver:
	- `create_plot(fig_counter, T_cold, T_hot, dt, u, n, fig)` – creates and configures a single subplot for time index `n` and returns the `AxesImage` (`im`).
	- `output_plots(im, fig)` – finalizes the figure (adds shared colorbar and shows the plot window).

Tips:

- Try changing `dx`, `dy` and `D` inside `diffusion2d.py` and observe how `dt` and runtime change. Smaller `dx`/`dy` increase the grid size and therefore the computation time.
- The script computes `dt` to satisfy the stability condition for the explicit scheme; if you change parameters, check `dt` to ensure stability.

## Exercises

Follow these exercise steps (as in the assignment):

1. Fork this repository and clone your fork locally.
2. Open `diffusion2d.py` and read through the code to understand the numerical method and the implementation.
3. Check that your Python version is >= 3.6 and update Python if necessary.
4. Install `pip`, `build` and `twine` if you plan to create a package.
5. Install NumPy and Matplotlib as described above.
6. Run the script with `python3 diffusion2d.py` and save the generated figure to your machine.
7. Experiment with `dx`, `dy` and `D` and answer the following questions:
	- How does `dt` change when you change `dx`/`dy`?
	- How does the runtime change when the grid is refined?
	- How does the appearance of the diffusion in the plots change?
8. (Optional) Add simple CLI options with `argparse` so the user can override `dx`, `dy` and `D` from the command line.

## Citing

If you reuse this example in teaching or publications, please cite the book chapter and the scipython example page:

- "Learning Scientific Programming with Python", chapter 7 (two-dimensional diffusion example).
- scipython.com: The two-dimensional diffusion equation (https://scipython.com/book/chapter-7-matplotlib/examples/the-two-dimensional-diffusion-equation/)
