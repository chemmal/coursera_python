# http://www.codeskulptor.org/#user47_cYbCtHe7jGxmxc4.py

import simplegui
import math
import random


# 1 deci_second == 100 milliseconds == 0.1 seconds

WIDTH, HEIGHT = 600, 600              # Frame width and height
CX, CY = (WIDTH // 2), (HEIGHT // 2)  # center of canvas
RADIUS = (WIDTH // 2) - 50            # Radius of analog clock

ticks = 0    # count of deci_seconds
deci_secs, secs, mins = 0, 0, 0

# How many times user tried to stop and managed to stop
# the stopwatch exactly at a whole second?
stop_tries, stop_wins = 0, 0

corona_points = []


# #############################################
def format(ticks):
    """Convert ticks (once every 100ms)
       into a string of format "M:SS.d"
       where 'M' is a single-digit-Minute
             'SS' is two digit seconds
             and 'd' is single-digit Deci-seconds
       and return it"""
    global deci_secs, secs, mins
    
    t = ticks % (10 * 60 * 10)
    deci_secs = t % 10
    t = t // 10
    
    secs = t % 60
    mins = t // 60
    
    return "%01d:%02d.%01d" % (mins, secs, deci_secs)

def radian(angle_in_degrees):
    """Convert degree to radians as math.cos() and math.sin() work
       only with radians. Also degree-0 falls in 3'o clock position.
       Subtract 90 degrees to offset it to 12'o clock position"""
    return (angle_in_degrees - 90) * (math.pi / 180.0)
    
def get_point_in_circle(cx, cy, angle, radius):
    x = int(cx + radius * math.cos(radian(angle)))
    y = int(cy + radius * math.sin(radian(angle)))  
    return x, y

def draw_clock(canvas, value, cx, cy, radius, splits,
               line_width=2, color="Green", fill_color=None,
               hand_width=3, hand_color="White"):
    canvas.draw_circle([cx, cy], radius, line_width,
                       color, fill_color)
    angle_step = 360 // splits
    for angle in range(0, 360, angle_step):
        x, y = get_point_in_circle(cx, cy, angle, radius - 10)
        canvas.draw_circle([x, y], 3, 1, "Yellow")
               
    # Convert the given value into a angled-line (clock-hand)
    value_angle = value * angle_step
    x2, y2 = get_point_in_circle(cx, cy, value_angle, radius - 15)
    canvas.draw_line([cx, cy], [x2, y2], hand_width, hand_color)
    
def draw_analog_clocks(canvas):
    # Draw the bigger, seconds clock
    draw_clock(canvas, secs, CX, CY, RADIUS, 60,
               line_width=4, color="Green", fill_color="Grey",
               hand_width=3, hand_color="Red")
    
    # Draw smaller, minute-clock
    min_cx, min_cy = get_point_in_circle(CX, CY, -45, RADIUS // 2)
    min_radius = RADIUS // 4 - 5
    draw_clock(canvas, mins, min_cx, min_cy, min_radius, 10,
               line_width=2, color="Yellow", fill_color="Green",
               hand_width=6, hand_color="Blue")
               
    # Draw smaller, deci_secs clock
    deci_cx, deci_cy = get_point_in_circle(CX, CY, 45, RADIUS // 2)
    deci_radius = RADIUS // 4 - 5
    draw_clock(canvas, deci_secs, deci_cx, deci_cy, deci_radius, 10,
               line_width=2, color="Yellow", fill_color="Green",
               hand_width=2, hand_color="Blue")
    
    draw_corona(canvas)
    draw_face_mask(canvas)
  
def get_corona_points():
    global corona_points
    corona_points = []
    for j in range(0, 48):
        angle = j * (360/48.0) + random.randint(0, 10)
        x, y = get_point_in_circle(CX, CY, angle, RADIUS+3)
        corona_points.append([x, y, angle])

def draw_corona(canvas):
    for x, y, angle in corona_points:
        canvas.draw_circle([x, y], 3, 1, "Brown", "Brown")
        
        for k in range(1, 6):
            rad = RADIUS + 3 * k + random.random()
            angle_range = [ angle + r * 0.33 * random.random() \
                            for r in range(-k, k+1) ]
            for angl in angle_range:
                x2, y2 = get_point_in_circle(CX, CY, angl, rad)
                canvas.draw_circle([x2, y2], 3, 1, "Brown", "Brown")
                
def draw_face_mask(canvas):
    mask_points = [(CX-155, CY+70), (CX, CY+50), (CX+155, CY+70),
                   (CX+143, CY+155), (CX, CY+178 ), (CX-143, CY+155) ]
    canvas.draw_polygon(mask_points, 12, "Blue", "Blue")
    
    clock_9_point = get_point_in_circle(CX, CY, 9 * (360/12), RADIUS)
    clock_8_point = get_point_in_circle(CX, CY, 8 * (360/12), RADIUS)
    clock_3_point = get_point_in_circle(CX, CY, 3 * (360/12), RADIUS)
    clock_4_point = get_point_in_circle(CX, CY, 4 * (360/12), RADIUS)
    
    canvas.draw_line(mask_points[0], clock_9_point, 2, "White")
    canvas.draw_line(mask_points[5], clock_8_point, 2, "White")
    canvas.draw_line(mask_points[2], clock_3_point, 2, "White")
    canvas.draw_line(mask_points[3], clock_4_point, 2, "White")
        
# #############################################

     
# #############################################
def tmr_h_every100ms():
    global ticks
    ticks += 1
    
def btn_h_start_stopwatch():
    global timer
    
    if not timer.is_running():
        timer.start()
    
def btn_h_stop_stopwatch():
    global timer, stop_tries, stop_wins
    
    if timer.is_running():
        timer.stop()
        stop_tries += 1
        if deci_secs == 0:
            stop_wins += 1
            # When user wins, virus mutates
            get_corona_points()
    
def btn_h_reset_stopwatch():
    global timer, ticks, stop_tries, stop_wins
    timer.stop()
    ticks = 0
    stop_tries, stop_wins = 0, 0
    
def btn_h_add_30secs():
    global ticks
    if timer.is_running():
        ticks += 30 * 10
    
def draw_h(canvas):
    draw_analog_clocks(canvas)

    clock_text = format(ticks)
    text_position = [CX - 75, CY + 35] 
    canvas.draw_text(clock_text, text_position, 60, "Cyan")
    
    canvas.draw_text("win%   wins / tries", [WIDTH - 150, 20], 20, "White")
    win_pct = stop_wins * 100.0/stop_tries if stop_tries > 0 else 0.0
    stop_win_text = "%6.02f%s    %02d / %02d" % (\
                    win_pct, '%', stop_wins, stop_tries) 
    canvas.draw_text(stop_win_text, [WIDTH - 160, 40], 20, "Orange")
    
    canvas.draw_text("Corona Virus StopWatch! Win to mutate virus!",
                     [CX - 190, HEIGHT-10], 20, "White")
# #############################################
    
get_corona_points()
    
frame = simplegui.create_frame("Corona Virus StopWatch", WIDTH, HEIGHT)
frame.set_draw_handler(draw_h)

btn_start = frame.add_button("Start", btn_h_start_stopwatch, 200)
btn_stop = frame.add_button("Stop", btn_h_stop_stopwatch, 200)

frame.add_label("")  # Just a spacer
frame.add_button("Reset", btn_h_reset_stopwatch, 200)
frame.add_button("Add 30 seconds", btn_h_add_30secs, 200)

frame.add_label("")  # Just a spacer
frame.add_label("Try stopping at a whole second\n"\
                "And win points to mutate virus!\n", 200)

timer = simplegui.create_timer(100, tmr_h_every100ms)

frame.start()
# timer.start()  # start/stop controlled by event-handlers
