```python
import numpy as np
from manim import (
    DEFAULT_FONT_SIZE,
    DOWN,
    GREEN,
    LEFT,
    PURPLE,
    RIGHT,
    SMALL_BUFF,
    UP,
    WHITE,
    ImplicitFunction,
    MathTex,
    NumberPlane,
    Rotate,
    Scene,
    Text,
    Vector,
    VGroup,
    Write,
    config,
)

config.pixel_height = 810
config.pixel_width = 1080
config.frame_height = 6.0
config.frame_width = 8.0


def lhs(x, y, alpha, v, w, a):
    k = w / v
    u = y / x
    return (x * (np.cos(alpha) - u * np.sin(alpha)) / (a * np.cos(alpha))) ** (-2 * k)


def rhs(x, y, alpha):
    u = y / x
    result = (1 - np.sin(alpha)) * (
        (u**2 + 1) ** 0.5 + np.sin(alpha) + u * np.cos(alpha)
    )
    result = result / (
        (1 + np.sin(alpha)) * ((u**2 + 1) ** 0.5 - np.sin(alpha) - u * np.cos(alpha))
    )
    return result


def f(x, y, alpha, v, w, a):
    return rhs(x, y, alpha=alpha) - lhs(x=x, y=y, alpha=alpha, v=v, w=w, a=a)


class ApplyMatrixExample(Scene):
    def construct(self):
        alpha = np.pi / 10
        ratio = 4.8 / 5
        W, A = 1.2, 6.8
        V = W / ratio
        X = 2.5
        graph = ImplicitFunction(
            lambda x, y: f(x, y, alpha, V, W, A),
            x_range=(X, A),
            y_range=(0, A),
            min_depth=8,
            max_quads=2000,
            color=GREEN,
        )
        og_number_plane = NumberPlane(
            x_range=(-16, 16, 1),
            y_range=(-16, 16, 0.5),
            x_length=32,
            y_length=32,
            background_line_style={"stroke_opacity": 0},
            axis_config={"stroke_width": 4},
        )
        wind = Vector([W * np.cos(np.pi / 2 - alpha), W * np.sin(np.pi / 2 - alpha)])
        wind.add(MathTex("\mathbf{w}").next_to(wind, RIGHT))
        wind.shift([X, graph.get_top()[1], 0])

        a_text = MathTex("a").move_to([A, -SMALL_BUFF, 0], aligned_edge=UP)

        # Create a group that will contain everything so that everything
        # move together
        objects = VGroup(graph, og_number_plane, wind, a_text)
        objects.shift((-3, -2.5, 0))

        number_plane = og_number_plane.copy()
        self.add(objects, number_plane)
        og_number_plane.set(color=PURPLE)

        # Calculate the new origin (center of the number plane)
        new_origin = og_number_plane.get_center()

        # Rotate the number plane around its new origin
        self.play(
            Rotate(number_plane, angle=-alpha, about_point=new_origin), run_time=3
        )
        self.wait(0.5)

        og_text = Text(
            "Original axes", color=PURPLE, font_size=int(DEFAULT_FONT_SIZE / 1.5)
        ).move_to(new_origin + np.array([1.5, SMALL_BUFF, 0]), aligned_edge=LEFT + DOWN)
        objects.add(og_text)
        self.play(Write(og_text))
        objects.add(number_plane)
        self.play(
            Rotate(objects, angle=alpha, about_point=new_origin),
            run_time=3,
        )
        new_text = Text(
            "New axes", color=WHITE, font_size=int(DEFAULT_FONT_SIZE / 1.5)
        ).move_to(new_origin + np.array([4, SMALL_BUFF, 0]), aligned_edge=LEFT + DOWN)
        self.play(Write(new_text))
        self.wait()

```

> [!blank-container|center-large]
> ![[rotation of ref.gif]]