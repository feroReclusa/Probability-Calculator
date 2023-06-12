# Probability-Calculator
import random

class Hat:
    def __init__(self, **kwargs):
        self.contents = []
        for color, quantity in kwargs.items():
            self.contents.extend([color] * quantity)

    def draw(self, num_balls):
        num_balls = min(num_balls, len(self.contents))
        balls_drawn = random.sample(self.contents, num_balls)
        for ball in balls_drawn:
            self.contents.remove(ball)
        return balls_drawn

def experiment(hat, expected_balls, num_balls_drawn, num_experiments):
    num_successful = 0

    for _ in range(num_experiments):
        hat_copy = hat.copy()
        balls_drawn = hat_copy.draw(num_balls_drawn)
        balls_drawn_dict = {}
        for ball in balls_drawn:
            balls_drawn_dict[ball] = balls_drawn_dict.get(ball, 0) + 1

        success = True
        for color, quantity in expected_balls.items():
            if color not in balls_drawn_dict or balls_drawn_dict[color] < quantity:
                success = False
                break

        if success:
            num_successful += 1

    probability = num_successful / num_experiments
    return probability
