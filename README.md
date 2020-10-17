# games
import turtle
import winsound


def run_game():
    wn = turtle.Screen()
    wn.title("pong by @maximus")
    wn.bgcolor("black")
    wn.setup(width=800, height=600)
    wn.tracer(0)

    # Paddle a
    paddle_a = turtle.Turtle()
    paddle_a.speed(0)
    paddle_a.shape("square")
    paddle_a.color("white")
    paddle_a.shapesize(stretch_wid=5, stretch_len=1)
    paddle_a.penup()
    paddle_a.goto(-350, 0)

    # Paddle b
    paddle_b = turtle.Turtle()
    paddle_b.speed(0)
    paddle_b.shape("square")
    paddle_b.color("white")
    paddle_b.shapesize(stretch_wid=5, stretch_len=1)
    paddle_b.penup()
    paddle_b.goto(350, 0)

    # ball
    ball = turtle.Turtle()
    ball.speed(0)
    ball.shape("square")
    ball.color("white")
    ball.penup()
    ball.goto(0, 0)
    ball.dx = 0.15
    ball.dy = 0.15

    # Pen
    pen = turtle.Turtle()
    pen.speed(0)
    pen.color("white")
    pen.penup()
    pen.hideturtle()
    pen.goto(0, 260)
    pen.write("Player A: 0 Player B: 0", align="center", font=("courier", 24, "normal"))

    # max score
    ms = turtle.Turtle()
    ms.speed(0)
    ms.color("white")
    ms.penup()
    ms.hideturtle()
    ms.goto(0, 220)

    # score
    score_a = 0
    score_b = 0

    # Pregame Setup
    numbers = wn.textinput("number of point to win", "please enter  max score")
    ms.write("Max score: {}".format(numbers), align='center', font=('courier', 24, "normal"))
    wn.textinput('info', 'keybind Player A: W/S & PLayer B: O/L press any key to continue')


    # Function
    def paddle_a_up():
        y = paddle_a.ycor()
        y += 20
        paddle_a.sety(y)

    def paddle_a_down():
        y = paddle_a.ycor()
        y += -20
        paddle_a.sety(y)

    def paddle_b_up():
        y = paddle_b.ycor()
        y += 20
        paddle_b.sety(y)

    def paddle_b_down():
        y = paddle_b.ycor()
        y += -20
        paddle_b.sety(y)

    # binding

    wn.listen()
    wn.onkeypress(paddle_a_up, "w")
    wn.onkeypress(paddle_a_down, "s")
    wn.onkeypress(paddle_b_up, "o")
    wn.onkeypress(paddle_b_down, "l")



    # main game loop
    while True:
        wn.update()

        # move the ball
        ball.setx(ball.xcor() + ball.dx)
        ball.sety(ball.ycor() + ball.dy)

        # border checking
        if ball.ycor() > 290:
            ball.sety(290)
            ball.dy *= -1
            winsound.PlaySound("66136__aji__ding30603-spedup.wav", winsound.SND_ASYNC)

        if ball.ycor() < -290:
            ball.sety(-290)
            ball.dy *= -1
            winsound.PlaySound("66136__aji__ding30603-spedup.wav", winsound.SND_ASYNC)

        if ball.xcor() > 390:
            ball.goto(0, 0)
            ball.dx *= -1
            score_a += 1
            pen.clear()
            pen.write("Player A: {} Player B: {}".format(score_a, score_b), align="center",
                      font=("courier", 24, "normal"))
            winsound.PlaySound("95557__robinhood76__01662-cartoon-jump.wav", winsound.SND_ASYNC)

        if ball.xcor() < -390:
            ball.goto(0, 0)
            ball.dx *= -1
            score_b += 1
            pen.clear()
            pen.write("Player A: {} Player B: {}".format(score_a, score_b), align="center",
                      font=("courier", 24, "normal"))
            winsound.PlaySound("95557__robinhood76__01662-cartoon-jump.wav", winsound.SND_ASYNC)

        # Paddle and ball collision
        if (ball.xcor() > 340 and ball.xcor() < 350) and (
                ball.ycor() < paddle_b.ycor() + 40 and ball.ycor() > paddle_b.ycor() - 40):
            ball.setx(340)
            ball.dx *= -1
            winsound.PlaySound("66136__aji__ding30603-spedup.wav", winsound.SND_ASYNC)

        if (ball.xcor() < -340 and ball.xcor() < -350) and (
                ball.ycor() < paddle_a.ycor() + 40 and ball.ycor() > paddle_a.ycor() - 40):
            ball.setx(-340)
            ball.dx *= -1
            winsound.PlaySound("66136__aji__ding30603-spedup.wav", winsound.SND_ASYNC)
        # Game AND Restart
        if score_b >= int(numbers):
            restart = wn.textinput("Well done plater B", " press 'Y' to continue or 'N' to close")
            if restart == "y":
                wn.clearscreen()
                run_game()
            else:
                break
        if score_a >= int(numbers):
            restart = wn.textinput("Well done player A", " press 'Y' to continue or 'N' to close")
            if restart == "y":
                wn.clearscreen()
                run_game()
            else:
                break


run_game()
