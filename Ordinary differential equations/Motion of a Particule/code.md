

The absolutely disgusting code for the gif of motion

```python
class FrictionalForce(Scene):
    def construct(self):
        theta = ValueTracker(0)

        surface = Line(start=[-4, 0, 0], end=[4, 0, 0])

        def sign(num):
            return -1 if num < 0 else 1

        def rotation_matrix():
            angle = theta.get_value()
            return np.array(
                [[np.cos(angle), -np.sin(angle)], [np.sin(angle), np.cos(angle)]]
            )

        def stretch_matrix(func, coeff=1):
            angle = theta.get_value()
            return np.array([[coeff * func(angle), 0], [0, coeff * func(angle)]])

        def rotate_line(start, end):
            angle = theta.get_value()
            start_hypothenus = (start[0] ** 2 + start[1] ** 2) ** 0.5
            end_hypothenus = (end[0] ** 2 + end[1] ** 2) ** 0.5

            start = [
                sign(start[0]) * start_hypothenus * math.cos(angle),
                sign(start[0]) * start_hypothenus * math.sin(angle),
                start[2],
            ]
            end = [
                sign(end[0]) * end_hypothenus * math.cos(angle),
                sign(end[0]) * end_hypothenus * math.sin(angle),
                end[2],
            ]
            return Line(start=start, end=end)

        def rotate_frictional_force(
            start, end, fix_end=False, buff=0, func=np.cos, coeff=1.0, color=WHITE
        ):
            rot = rotation_matrix()
            stretch = stretch_matrix(func, coeff=coeff)
            start = np.array(start)
            new_start = np.dot(rot, np.array(start[:2]))
            if fix_end:
                new_end = np.array(end)
            else:
                end = np.array(end)
                if color != BLUE:
                    new_end = (
                        np.dot(stretch, np.dot(rot, end[:2] - start[:2]))
                        + new_start[:2]
                    )
                else:
                    new_end = (
                        -coeff
                        * np.sin(PI + theta.get_value())
                        * np.array(
                            [
                                np.cos(PI + theta.get_value()),
                                np.sin(PI + theta.get_value()),
                            ]
                        )
                    ) + new_start[:2]

            start = [
                new_start[0],
                new_start[1],
                start[2],
            ]
            end = [
                new_end[0],
                new_end[1],
                end[2],
            ]
            return Arrow(start=start, end=end, buff=buff, color=color)

        surface = always_redraw(lambda: rotate_line([-5, 0, 0], [5, 0, 0]))
        friction = always_redraw(
            lambda: rotate_frictional_force([1, 1, 0], [2, 1, 0], color=GREEN)
        )
        gravity = Vector(DOWN * 3, color=GOLD)
        normal = always_redraw(
            lambda: rotate_frictional_force(ORIGIN, DOWN * 3, buff=0, color=GREEN)
        )
        fx = Arrow(DOWN * 3, DOWN * 3, color=BLUE)
        fx.add_updater(
            lambda m: m.become(
                Arrow(normal.get_end(), gravity.get_end(), buff=0, color=BLUE)
            )
        )
        body = Rectangle(height=UP[1] * 1.5, width=RIGHT[0] * 2).next_to(surface, UP)
        body.add(MathTex("m").move_to(body.get_center()))
        body_motion = always_redraw(
            lambda: rotate_frictional_force(
                [-1, 1, 0],
                [-1, 1, 0],
                func=lambda x: np.sin(x),
                coeff=3,
                color=BLUE,
            )
        )

        gravity_text = MathTex(r"\vec{F} = mg", color=GOLD).next_to(gravity, LEFT)

        friction_value = MathTex(
            r"\mu\left(mg\cos({})\right)".format(theta.get_value()), color=GREEN
        ).move_to(friction.get_end() + 2 * RIGHT)
        friction_value.add_updater(
            lambda fv: fv.become(
                MathTex(
                    r"\mu\left(mg\cos({:.2f})\right)".format(theta.get_value()),
                    color=GREEN,
                ).move_to(friction.get_end() + 2 * RIGHT)
            )
        )

        fx_text = MathTex(r"mg\sin({})".format(theta.get_value()), color=BLUE).next_to(
            body_motion, LEFT
        )
        fx_text.add_updater(
            lambda f: f.become(
                MathTex(
                    r"mg\sin({:.2f})".format(theta.get_value()), color=BLUE
                ).next_to(body_motion, LEFT)
            )
        )

        fy_text = MathTex(r"mg\cos({})".format(theta.get_value()), color=GREEN).move_to(
            normal.get_end() + 2 * RIGHT
        )
        fy_text.add_updater(
            lambda fv: fv.become(
                MathTex(
                    r"mg\cos({:.2f})".format(theta.get_value()), color=GREEN
                ).move_to(
                    normal.get_end() + 2 * RIGHT,
                )
            )
        )

        angle_text = MathTex(r"\theta = {:.2f}".format(theta.get_value())).to_edge(UL)
        angle_text.add_updater(
            lambda fv: fv.become(
                MathTex(r"\theta = {:.2f}".format(theta.get_value())).to_edge(UL)
            )
        )

        self.add(
            surface,
            friction,
            normal,
            fx,
            body_motion,
            gravity,
            body,
            friction_value,
            fy_text,
            gravity_text,
            fx_text,
            angle_text,
        )
        self.play(
            AnimationGroup(
                theta.animate.set_value(PI / 4),
                Rotate(
                    body,
                    angle=PI / 4,
                    about_point=ORIGIN,
                ),
                lag_ratio=0,
            ),
            run_time=2,
        )
        self.wait()
        self.play(
            AnimationGroup(
                theta.animate.set_value(0),
                Rotate(
                    body,
                    angle=-PI / 4,
                    about_point=ORIGIN,
                ),
                lag_ratio=0,
            ),
            run_time=2,
        )
        self.wait()

        self.play(
            AnimationGroup(
                theta.animate.set_value(PI / 2),
                Rotate(
                    body,
                    angle=PI / 2,
                    about_point=ORIGIN,
                ),
                lag_ratio=0,
            ),
            run_time=2,
        )
        self.wait()
        self.play(
            AnimationGroup(
                theta.animate.set_value(0),
                Rotate(
                    body,
                    angle=-PI / 2,
                    about_point=ORIGIN,
                ),
                lag_ratio=0,
            ),
            run_time=2,
        )
        self.wait()
        return self

```

![[Motion of a Particule.gif]]