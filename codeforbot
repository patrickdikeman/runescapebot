import time
import pyautogui
import numpy as np
import matplotlib
import cv2
import os
from PIL import ImageGrab, Image
import os
import time
import matplotlib.pyplot as plt
import imutils
import sys
import random
from functools import reduce

#im = pyautogui.screenshot('cat.png', region=(842,215,220,205))

# coordinates = []
# for x in range(10,220,10):
#     for y in range(10,210,10):
#         coordinates.append((x,y))
#
# print (coordinates[0])


def regionworks(target_image, screen_x, screen_y, width, height):
    """
        target_image is the filename of a target image
        screen_x is the x coordinate of the top left /pixel/ of the runescape window on your screen
        screen_y is the y coord of the blah blah
        width is the width in pixels of your target image
        same with height
    """
    screenshot = pyautogui.screenshot()
    target_image_pyimage_obj = Image.open(target_image)
    pixels = target_image_pyimage_obj.load()

    for x in range(0, width):
        for y in range(0, height):
            rgb = pixels[x][y]

            if not screenshot.getpixel((screen_x + x, screen_y + y, rgb)) is rgb:
                return False
    return True


def is_color_pixel_in_area(area_x, area_y, width, height, color):
    """
        parameters:
        area_x: the x coord of the top left pixel of the area on the screen that you would like to screenshot
        area_y: same but for y
        width: the width on the x plane, in pixels
        height: same
        color: a tuple representing the rgb value of the target color, in the form (r,g,b)

        returns:
        either a (x,y) coordinate pair of the place where teh color was found respecting the entire screen, or false if it wasn't found
    """
    starting_time = time.clock()
    screenshot = pyautogui.screenshot(region=(area_x, area_y, width, height))
    for x in range(0, width):
        for y in range(0,height):
            pixel_rgb = screenshot.getpixel((x,y))
            if is_equal_with_wiggle_room(pixel_rgb,color,5):
                print("returning a place in %f seconds" % (time.clock() - starting_time))
                return (area_x + x, area_y + y)
    print("returning False in %f seconds" % (time.clock() - starting_time))
    return False

def is_equal_with_wiggle_room(rgb1,rgb2,wiggle_room):
    r1, g1, b1 = rgb1
    r2, g2, b2 = rgb2

    tolerance = range(-wiggle_room, wiggle_room + 1)

    if((r1 - r2) in tolerance):
        if((g1 - g2) in tolerance):
            if((b1 - b2) in tolerance):
                return True
    return False

def update_relative_position(absolute, relative, new_abs_x, new_abs_y):
    abs_x, abs_y = absolute
    rel_x, rel_y = relative

    delta_x = abs_x - new_abs_x
    delta_y = abs_y - new_abs_y

    return (rel_x + delta_x, rel_y + delta_y)

def clamp_within(starting_position, current_rel_pos, desired_click_position_absolute, allowed_square_side):
    des_x, des_y = desired_click_position_absolute
    curr_rel_x, curr_rel_y = current_rel_pos
    start_x, start_y = starting_position

    final_x = des_x
    final_y = des_y

    # if((curr_rel_y + (start_y - des_y)) < allowed_square_side / 2):
    #     final_y = start_y

    # if(des_x > start_x + allowed_square_side / 2 ):
    #     final_x = int(start_x + allowed_square_side / 2 )
    # elif(des_x < start_x - allowed_square_side / 2):
    #     final_x = int(start_x - allowed_square_side / 2 )
    #
    # if(des_y > start_y + allowed_square_side / 2 ):
    #     final_y = int(start_y + allowed_square_side / 2 )
    # elif(des_y < start_y - allowed_square_side / 2):
    #     final_y = int(start_y - allowed_square_side / 2 )
    #
    return (final_x,final_y)



if __name__ == "__main__":

    absolute_position_on_screen = (960,560)
    current_position_relative_to_starting_position = (0,0)
    allowed_square_side = 200
    # this is the width of the square region around the character that will be screenshot
    offset = 300
    min_transit_time = 0.5
    max_transit_time = 1.5
    green = (41,54,24)
    # this is the distance that the new click position could be from the character
    max_search_click = 200
    min_search_click = -100

    max_non_found_enemy_wait_time = 3
    min_non_found_enemy_wait_time = 1



    easing_methods = [pyautogui.easeInQuad,pyautogui.easeOutQuad,pyautogui.easeInOutQuad,pyautogui.easeInBounce,pyautogui.easeInElastic]
    if sys.argv[1] == "test":
        x = absolute_position_on_screen[0] - offset
        y = absolute_position_on_screen[1] - offset
        side_len = offset * 2
        print("get your screen ready!")
        time.sleep(2)
        print("beginning!")
        while(True):
            maybe = is_color_pixel_in_area(x,y,side_len,side_len,green)
            # print(maybe)
            if maybe:
                print("found an enemy, clicking on it")
                transit_time = random.random() * (max_transit_time - 1) + min_transit_time
                found_green_x, found_green_y = maybe

                move_x, move_y = clamp_within(absolute_position_on_screen,current_position_relative_to_starting_position,(found_green_x,found_green_y),allowed_square_side)
                # current_position_relative_to_starting_position = update_relative_position(absolute_position_on_screen,current_position_relative_to_starting_position,move_x,move_y)
                print("current relative position: ")
                print(current_position_relative_to_starting_position)
                pyautogui.moveTo(move_x, move_y, transit_time, random.choice(easing_methods))
                pyautogui.click()
                time.sleep(2)
            else:
                print("no enemies found")
                transit_time = random.random() * (max_transit_time - 1) + min_transit_time
                new_x = absolute_position_on_screen[0]
                new_y = absolute_position_on_screen[1] +  20

                #move_x, move_y = clamp_within(absolute_position_on_screen,current_position_relative_to_starting_position,(new_x,new_y),allowed_square_side)
                #current_position_relative_to_starting_position = update_relative_position(absolute_position_on_screen,current_position_relative_to_starting_position,move_x,move_y)
                #print("current relative position: ")
                #print(current_position_relative_to_starting_position)
                #pyautogui.moveTo(move_x, move_y, transit_time, random.choice(easing_methods))
                #pyautogui.click()


                time.sleep(random.random() * (max_non_found_enemy_wait_time - 1) + min_non_found_enemy_wait_time)

                # give us some time to walk to the new place
                # time.sleep(5)
                # time.sleep(random.choice(range(1,6)))
