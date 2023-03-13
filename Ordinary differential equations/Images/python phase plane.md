This bit of code sould take care of drawing a phase plane. Change the matrix $\mathbf{A}$ for different ODE. The rest can stay the same.

```bash
conda create -n ode python=3.9 ipython matplotlib
```

```python
from pylab import grid, show, savefig, subplots, tight_layout, style, rcParams
import numpy as np
from itertools import chain

style.use("bmh")
rcParams["text.usetex"] = True
rcParams["text.latex.preamble"] = r"\usepackage{{amsmath}}"


def phase_plane(A, x_min=-3, x_max=3, y_min=-3, y_max=3, ax=None):
    if ax is None:
        fig, ax = subplots(1, 1, figsize=(5, 5), dpi=500)
    ax.set_title(
        r"$\mathbf{A} = \begin{bmatrix} %s & %s \\ %s & %s \end{bmatrix}$"
        % (A[0, 0], A[0, 1], A[1, 0], A[1, 1])
    )
    state_vec = np.array(
        np.meshgrid(np.linspace(x_min, x_max, 1000), np.linspace(y_min, y_max, 1000))
    )
    state_dot = A @ np.transpose(state_vec, (1, 0, 2))

    # plot the phase plane
    ax.streamplot(
        state_vec[0, :, :],
        state_vec[1, :, :],
        state_dot[:, 0],
        state_dot[:, 1],
        linewidth=1,
        arrowsize=1,
        density=0.5,
    )

    # plot the eigen planes
    eigvalues, eigvectors = np.linalg.eig(A)

    colors = ["orange", "green"]
    # if the eigen values and vectors are not complex, plot them.
    if not eigvalues.dtype.str.startswith("<c"):
        for i in range(2):
            # eigen plane
            vec = eigvectors[:, i]
            idx = np.where(vec == 0)[0]
            if len(idx) == 0:
                m = vec[0] / vec[1]
                x = np.array([x_min, x_max])
                y = x * m
                ax.plot(x, y, lw=2, color=colors[i])
            else:  # special case where the vector is one of the axis
                if idx == 0:
                    ax.plot([0, 0], [y_min, y_max], lw=2, color=colors[i])
                else:
                    ax.plot([x_min, x_max], [0, 0], lw=2, color=colors[i])

            # eigen vectors (just the arrow head)
            for flip in [-1, 1]:
                direction = 1 if eigvalues[i] > 0 else -1
                vec_ = vec * flip
                ax.arrow(
                    *vec_,
                    *(direction * np.sign(vec_) * np.array([0.01, 0.01])),
                    width=0.01,
                    head_width=0.1,
                    color=colors[i],
                    lw=2,
                    overhang=0.4
                )

        if np.abs(eigvalues[0]) != np.abs(eigvalues[1]):
            highest_eigval = np.argmax(np.abs(eigvalues))
            vec = eigvectors[:, highest_eigval]
            for flip in [-1, 1]:
                vec_ = vec * flip
                direction = 1 if eigvalues[highest_eigval] > 0 else -1
                ax.arrow(
                    *(vec_ * 0.8),
                    *(direction * np.sign(vec_) * np.array([0.01, 0.01])),
                    width=0.01,
                    head_width=0.1,
                    color=colors[highest_eigval],
                    lw=2,
                    overhang=0.4
                )
    grid()
    tight_layout(pad=1.0)



# Change this matrix to plot other ODE
source = np.array(
    [
        [3, -1],
        [-1, 3]
    ]
)

phase_plane(source)
savefig('source.png')

sink = -source
phase_plane(sink)
savefig('sink.png')

saddle = np.array(
    [
        [1, 0],
        [0, -1]
    ]
)
phase_plane(saddle)
savefig('saddle.png')

spiral_in = np.array(
    [
        [-1, 2],
        [-2, -1]
    ]
)
phase_plane(spiral_in)
savefig('spiral_in.png')

spiral_out = np.array(
    [
        [1, 2],
        [-2, 1]
    ]
)
phase_plane(spiral_out)
savefig('spiral_out.png')

show()
```

